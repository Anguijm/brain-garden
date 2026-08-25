---
title: "How the vault actually runs"
type: reference
tags: [vault, engine, workflow, newsletter, research, automation]
created: 2026-08-26
updated: 2026-08-26
---

# How the vault actually runs

This is the sequence map: what happens when a newsletter gets made, what happens when a topic gets
researched, and which parts are automatic versus which parts are a person or a model deciding something.

Everything below was read out of the scripts on 2026-08-26 rather than remembered, and the file names are
given so any of it can be checked.

## The thing to understand first

**The vault has almost no autonomous agents.** It has three things that are easy to confuse:

1. **Scripts.** Ordinary Python that fetches, filters, ranks, renders and checks. Most of the engine is
   this, and most of it makes no model call at all.
2. **Narrow model calls inside those scripts.** Five of them, listed further down. Each does one job:
   write a search query, draft a synthesis, flag a suspect claim, judge a flagged claim, criticise a
   finished issue.
3. **The session.** A Claude Code conversation doing the editorial judgement between script runs. This is
   where selection, argument and prose come from.

Nothing in the vault writes an article and publishes it. `expand_newsletter.py` says so in its own header:
it "gathers and stages only; the synthesis into the four newsletter sections is an editorial job done at
the gate (by the session), never auto-published."

## What runs on a timer

Three cron entries touch this vault.

| When | What | Cost | Output |
|---|---|---|---|
| Daily 03:30 | `light_gather.py` | Free | `00_inbox/light-scan.md` |
| Thursday 05:30 | `nsi_watch.py --report` | Free | `00_inbox/nsi-revisions.md`, only if something changed |
| Friday 04:41 | `expand_newsletter.py` | Paid | `10_staging/newsletter-<week>/` — **currently commented out** |

`light_gather.py` reads 15 RSS feeds, filters headlines on a keyword list, and flags anything whose
subject already appeared in a past issue. It imports no model client at all, and a test
(`test_light_gather.py`) asserts it never reaches `lib.searcher`, `anthropic` or `lib.usage`. That is why
it costs nothing and can run every day.

`nsi_watch.py` screens NAVSEA Standard Items against the published corpus. Silence is the normal weekly
result. A file appearing means something actually changed.

The Friday paid gather is switched off at the moment because the free daily scan has been carrying the
newsletter. Turning it back on is one line in the crontab.

## Loop one: making a newsletter issue

```
  [timer]   light_gather.py  ->  00_inbox/light-scan.md
                                   (15 feeds, keyword filter, repeat-subject flags)
      |
      v
  [session] read the candidates, apply the two gates before drafting:
              1. is it contracting mechanics?   -> kill
              2. has the brief already run it?  -> kill, unless it is
                                                   "the thing we told you to watch has happened"
      |
      v
  [session] pick four to six stories
      |
      v
  [script]  deepen_story.py "<story>"   (optional, paid)
              four search angles: primary / official / precedent / counter
              an empty counter result is a weak search, NOT evidence of no counter-argument
      |
      v
  [session] read the sources and WRITE the issue
              -> 10_staging/newsletter-<week>/<week>-v1.md
      |
      v
  [script]  publish_newsletter.py <issue.md> --out <pdf>
              hard lints: every FACT attributed in-sentence, a Bottom line and a
              Sources line per article, deep links only, colophon present
              then fits the type to exactly 4 article pages + 1 events page
      |
      v
  [model]   critique_issue.py <issue.md> --worksheet <answers.md>
              a DIFFERENT model reads the draft and attacks it
      |
      v
  [session] answer EVERY objection in writing: FIXED / DISPUTED / ACCEPTED
              then rewrite. Answer in a rewrite pass, not by patching sentences.
      |
      v
  [script]  critique_issue.py <issue.md> --check <answers.md>     (free, no model call)
      |
      v
  [operator] says the issue is done
      |
      v
  [session] move 10_staging/ -> 20_curated/projects/waterfront-brief/,
            list it on the series index
      |
      v
  [script]  publish_garden.sh      (the gate, below)
```

The loop can run several times between the render and the critique. Issue No. 5 reached v16. Rendering is
free; a critique pass costs about a dollar, which is why the critic runs at publish rather than at every
render.

**A draft is not published work.** An issue stays in `10_staging/newsletter-<week>/` until the operator
says it is done. Moving it into `20_curated/` publishes it, because the garden sync ships everything there.
`release_gate.py` blocks a publish if an unlisted issue is sitting in curated.

## Loop two: researching a brain topic

```
  [operator] gives a topic (typed here, or sent to the Signal group)
      |
      v
  [script]  expand.py "topic"
              |
              +- [model] write search queries              (label: search)
              +- [script] gather results, rank them
              +- [script] ingest.py each source            -> 10_staging/<topic>/sources/
              |             --render auto | fast | headed
              |             headed = visible Chromium on DISPLAY=:1 with stealth,
              |             for bot walls; real captchas are quarantined, not ingested
              +- [model] draft a labelled synthesis        (label: expand)
                          -> 10_staging/<topic>/synthesis.md
                             with a source ledger and open questions
      |
      v
  [session] read the sources, check the claims, rewrite honestly.
            Everything a model produced is Assessment or Speculation by default.
            FACT is only for what was verified against a primary source.
      |
      v
  [session] promote the keepers -> 20_curated/topics/<category>/<topic>/
      |
      v
  [script]  publish_garden.sh      (the gate, below)
      |
      v
  [session] reply with the garden link (over Signal, if that is where it came from)
```

For a single URL rather than a whole topic:
`ingest.py "<url>" --topic <t>`.

Government sources do not go on a wanted list, because law and filings are public and machine-readable.
`lib/routing.py` sends eCFR, SAM, congress.gov, govinfo, GAO and the Federal Register to real APIs. A few
hosts (`cbo.gov`, `commerce.gov`, `secnav.navy.mil`, `navsea.navy.mil`) refuse this network outright; the
answer there is a different publisher of the same record, and `BLOCKED_HOST_ALTERNATES` carries the known
ones.

## Every model call in the vault, and what it is for

Five call sites. Each is labelled, and the label is what shows up in the cost report.

| Label | Where | Job | Model |
|---|---|---|---|
| `search` | `lib/searcher.py` | Turn a topic into search queries and rank results | Claude or Gemini |
| `expand` | `expand.py` | Draft the staged synthesis | Claude |
| `ground-check-finder` | `ground_check.py` | Read a note against its sources and flag suspect FACTs | Cheap model, 120k-char budget |
| `ground-check-arbiter` | `ground_check.py` | Re-read every flagged claim at full budget and rule on it | Expensive model |
| `critique` | `critique_issue.py` | Attack a finished newsletter draft | A deliberately different model |

The two-stage grounding split is the design worth knowing: **the finder over-flags on purpose.** It is
cheap and high volume, and everything it flags is re-read by the arbiter before anything blocks a publish.
So a finder flag is not a verdict, and the arbiter is where the money and the judgement are.

`deepen_story.py` uses the `search` call four times, once per angle.

## The publish gate, in order

`publish_garden.sh` runs these before anything ships. Any one of them aborts the publish.

1. Rebuild the source register.
2. Validate curated frontmatter.
3. Check no alias collides with its own slug.
4. Run the regression suite (`run_checks.py --quick`).
5. Grounding pass on changed curated notes. Currently scoped to `projects/waterfront-brief/`; set
   `VAULT_GROUND_ALL=1` to include the rest. If the checker itself cannot run, the publish aborts rather
   than let a broken check look like a clean one.
6. `check_citations.py` — every cited URL exists, and no FACT rests on a page nobody fetched.
7. `release_gate.py check-released` — no unreleased issue parked in curated.
8. `release_gate.py check-critique` — every released issue answered its critic, no blank responses.
9. Print-drift — no issue `.md` newer than its PDF.
10. FACT-label lint (warn-only).
11. rsync `20_curated/` into the garden.
12. `release_gate.py prune-pdfs` — withhold any PDF whose issue note is absent or `draft: true`.
13. Escape `$` for the web build's math parser, then push.

Gate 8 exists because for a while nothing enforced it: the critic was convention only, and the worksheet
header claimed a blank response would block the render when nothing did.

## Seeing what it cost

Every run writes a manifest to `90_meta/runs/` with tokens, cached tokens, searches and dollars, split by
model and by call-site label. `cost_report.py --budget N` reads the spend ledger and prints the split plus
a **cache hit rate**.

That hit rate is the alarm. It was structurally 0% until 2026-08-06, which is how uncached input became
82% of a \$264 month. If it falls back toward zero, caching has broken and the bill roughly doubles before
anything else looks wrong.

**One exception, so it is not chased twice:** grounding a newsletter issue reads 0% by design. Caching
needs one source pool shared across a note's sections, and an issue gives every article its own sources,
so no pool is ever reused. Judge the alarm on curated-note runs.

Every run also has a hard ceiling (`VAULT_RUN_BUDGET_USD`, default \$15) checked before each call, so a
loop cannot drain the balance.

## Where things live

| Path | What |
|---|---|
| `00_inbox/` | Fast capture and timer output, pre-staging |
| `10_staging/<topic>/` | Sources plus a synthesis. Unverified. Drafts live here |
| `20_curated/` | Promoted, trusted notes. **Everything here publishes to the garden** |
| `90_meta/` | Rules, run manifests, the defect ledger |
| `_scripts/` | The engine. Run Python with `_scripts/.venv/bin/python3` |
| `_library/research` | The defence vault, read-only, never a source for anything published here |

## See also

- [How The Waterfront Brief is made](projects/waterfront-brief/how-it-works) — the editorial method:
  what it searches for, what it screens against, how a fact gets checked.
