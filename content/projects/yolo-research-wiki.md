---
type: project
title: "The YOLO research wiki"
status: curated
created: 2026-10-15
tags: [projects, yolo, ai-agents, loops, wiki, youtube]
---

<!-- Promoted 2026-10-15 after the operator's ruling of the same day ("Go host that wiki
     somewhere, make it a link to the brain"). The full record with the evidence is in
     10_staging/yolo-wiki/synthesis.md (private); this is the public pointer. -->

# The YOLO research wiki

> A nightly loop reads 13 YouTube channels about building with AI, turns each new video
> into a card, and a second loop files the cards into a wiki. The wiki is now hosted at
> https://anguijm.github.io/llm-wiki-hub/ and this is its door from the garden.

## What it is

The YOLO projects repo began in 2026 as a set of autonomous overnight builds (single-file
web apps, games, tools) and grew a research loop: a GitHub Actions workflow that runs
twice a day, reads the RSS feeds of 13 YouTube channels, fetches transcripts, and writes a
card for each new video with the Anthropic model doing the reading. A second repo, the
wiki hub, mirrors those cards into a personal wiki in the style of Andrej Karpathy's
"LLM wiki" idea, with links between cards, channels and topics.

FACT: from the 171 nightly run reports committed to the YOLO repo and read on
2026-10-15, the loop has found 2,147 videos and written 1,107 cards, 119 of them in
October 2026, the last 16 on 2026-10-13 (the vault's record of 2026-10-15, section 2).
FACT: the wiki build on 2026-10-15 rendered 1,138 pages, with an experiments index
linking 1,107 of them (same record, section 3).

## Where it lives

- The wiki: https://anguijm.github.io/llm-wiki-hub/ (rebuilt after each nightly sync).
- The experiments index: https://anguijm.github.io/llm-wiki-hub/experiments-index/
- The research loop: https://github.com/Anguijm/yolo-projects
- The wiki source: https://github.com/Anguijm/llm-wiki-hub

## What it is doing right now, honestly

FACT: all 13 feed addresses answered HTTP 200 from a home connection on 2026-10-15, and
all 13 channel pages exist (same record, section 4). FACT: the nightly runs on GitHub's
runners lost every feed on 2026-10-14 and 2026-10-15, lost some on 2026-10-06, 10-12 and
10-13, and had two earlier blank nights on 2026-08-28 and 08-29 (same record, section 4).
Assessment: the pattern, 404 and 500 answers for the same addresses on the same night
with a different set of surviving channels each run, reads as YouTube refusing requests
from GitHub's shared runner addresses, not as the wrong channels. The runs showed green
through all of it because the feed reader swallows every error and no step fails, which
is the state failure chapter 23 calls lost: the loop kept going and nothing noticed
that its input was empty.

Three of the thirteen channels have never produced a card, and about half of all cards
come from one conference channel, so "are we watching the right channels" is a fair
question and is open (same record, sections 4 and 6).

## See also

- **[Loops, graphs, and the state between them](topics/ai-engineering/23-loops-graphs-and-state)**: the chapter whose failure list this loop illustrates.
- **[AI engineering series](topics/ai-engineering/)**: the series this project sits beside.
