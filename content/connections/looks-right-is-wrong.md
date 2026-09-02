---
title: "Thread: when the artifact looks right and isn't"
status: curated
tags: [connections, moc, verification, trust]
created: 2026-09-03
---

# When the artifact looks right and isn't

Five domains, one failure. Something is produced that is **well-formed, plausible and wrong**, and
because it is well-formed, every cheap check passes. The expensive part is never producing the
thing; it is knowing whether to believe it.

Each note below hits this and answers it the same way: **check against an authoritative record, not
against appearances.**

## The thread

- **[Game guides and the ROM library](topics/games/batocera-top-games)** — a scraper labelled
  *Mega Man 2* as **Totally Rad**, complete with that game's description and box art. Nothing looked
  broken: there was a name, a cover, a plausible blurb. The cause was upstream — the scraper matches
  on filename, and the filename was in an old naming style. The fix was to stop trusting names and
  derive them from **checksums against the published dump records**, which found 26 misnamed files
  and 20 wrong entries.

- **[Verification rules](areas/how-the-vault-runs)** — this vault's whole labelling discipline
  exists for the same reason. Anything a model produces is **Assessment or Speculation by default**,
  and FACT is reserved for what was checked against a primary source. A fluent paragraph and a
  correct paragraph are indistinguishable at a glance, so the label carries what the prose cannot.

- **[AI engineering: evaluation](topics/ai-engineering/)** — the same problem stated formally. A
  model's confidence is uncorrelated with its accuracy, so plausibility cannot be the acceptance
  test. You need a held-out answer to compare against, which is a checksum by another name.

- **[The Waterfront Brief](projects/waterfront-brief/)** — a search result's summary asserted a
  publication date that the article's own page contradicted, once by two years. The summary was
  well-formed and wrong. The standing rule that came out of it: **open the page; a search summary is
  never a source.**

- **[AI stock-picking services, investigated](topics/finance/ai-stock-picking/)** — vendors publish
  backtested returns that are internally consistent, professionally presented, and unfalsifiable
  without the underlying trade log. The presentation quality carries no information about the
  result's validity, which is precisely why it is offered.

## What the pattern actually is

**A check that inspects the artifact will pass; only a check against an independent record fails.**

- A file listing said the download was complete. A checksum said 23 of the files were fragments.
- An HTTP 200 said the archive was alive. Reading the 2 KB body would have shown a closure notice.
- A box art image said the game was Mega Man 2. The SHA-1 said the ROM was Mega Man 2 and the
  *label* was Totally Rad.
- A fluent Assessment reads like a finding. Only the cited source settles whether it is one.

**The corollary is uncomfortable:** the better the generator, the less useful your intuition
becomes as a filter, because the failures stop looking like failures. That argues for cheap
mechanical checks run always, rather than careful human review run sometimes.

**And silence is not evidence.** An empty result meant "not available" six times in one session and
was wrong every time — the search API simply capped its response. A tool that fails by returning
nothing is more dangerous than one that fails loudly, because nothing looks like an answer.
