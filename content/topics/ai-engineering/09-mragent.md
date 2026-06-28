---
type: lesson
series: ai-engineering
chapter: 9
title: "MRAgent: reconstructive memory"
status: curated
tags: [ai, ai-agents, llm, agent-engineering, memory, mragent]
created: 2026-06-28
---

# Chapter 9 — MRAgent: reconstructive memory

MRAgent is a 2026 research framework for agent memory whose central claim is in the
title of its paper: *memory is reconstructed, not retrieved*. Instead of looking up a
fixed set of memory chunks and then reasoning over them, MRAgent reasons *while* it
walks an associative graph, deciding where to go next based on what it has found so
far. This chapter is the dedicated deep dive promised in [the memory
chapter](03-memory-for-agents): what the name means, how it works, the efficiency
numbers, and the limits.

## What it is, and what the name means

FACT: the framework is named **MRAgent**, introduced in the paper as "a framework that
combines an associative memory graph with an active reconstruction mechanism." The
abbreviation `MR` stands for **Reconstructive Memory**: the paper's Section 4 is titled
"MRAgent: Reconstructive Memory Agent," and its conclusion calls it "a reconstructive
memory agent." (arXiv:2606.06036, *Memory is Reconstructed, Not Retrieved: Graph
Memory for LLM Agents*, verified against the arXiv full text.)

A caution worth stating plainly, because the wrong name is everywhere. Assessment:
several AI-generated summaries claim `MR` stands for "Memory Reasoning Architecture for
LLM Agents." That phrase does not appear anywhere in the paper; it is a recurring
machine-summary fabrication. The correct expansion is **Reconstructive Memory Agent**.
This is itself a small lesson in the [hallucination problem](08-safety-and-best-practices):
a confident, widely repeated, and simply false claim.

FACT: the authors are **Shuo Ji, Yibo Li, and Bryan Hooi** at the National University
of Singapore; the paper was submitted on 4 June 2026. The code is at
`github.com/Ji-shuo/MRAgent` (not to be confused with an unrelated biomedical
`MRAgent` repo). Assessment: the publication venue is not reliably confirmed, sources
conflict between an ICLR workshop and ICML, so the safe citation is the arXiv paper and
its OpenReview record, without asserting acceptance anywhere specific.

## The core idea: reconstruct, do not retrieve

FACT: standard memory agents use a static "retrieve-then-reason" paradigm, which the
paper argues "prevents them from dynamically adapting memory access to intermediate
evidence discovered during inference." In that paradigm retrieval is "passive,
selecting memory units as a fixed function of the query without reasoning over
intermediate evidence." (arXiv:2606.06036.)

FACT: MRAgent instead makes memory access "an active, multi-step reconstruction
process." It "integrates LLM reasoning directly into memory access, allowing the agent
to iteratively explore and prune retrieval paths based on accumulated evidence."
Intermediate findings are "transformed into new retrieval constraints, allowing the
agent to recover evidence that is unreachable under passive policies." The design is
motivated by cognitive neuroscience: human memory is reconstructive, an act of
rebuilding, rather than a lookup. (arXiv:2606.06036.)

## The structure: a Cue-Tag-Content graph

FACT: memory is stored as a **Cue-Tag-Content graph**, "where associative tags serve as
semantic bridges connecting fine-grained cues to memory contents." A *cue* is a
fine-grained keyword such as an entity or attribute; a *content* node is a specific
memory item; and a *tag* summarizes the associative relation between them. These are
linked as triplets (cue, tag, content). (arXiv:2606.06036.)

![Diagram: three columns of nodes (cues, tags, content) linked as triplets; a highlighted green path traverses from a cue through an associative tag to a memory item, contrasted at the bottom with retrieve-then-reason versus reconstruct](img/cue-tag-content.png)
*The Cue-Tag-Content graph, with one reconstruction path highlighted. Diagram.*

FACT: the graph also has multi-granular memory layers (concrete events, stable facts,
and higher-level topics), and it is built by "memory population via LLM distillation,"
which rewrites dialogue turns into self-contained sentences and extracts keywords.
(arXiv:2606.06036; project README.)

## How a query is answered

FACT: answering a query is an iterative loop over a "reconstruction state" with
traversal actions: the LLM reasons over the query plus the evidence gathered so far and
selects promising directions to expand (reducing noise); it traverses the graph guided
by those selected actions "rather than exhaustive graph expansion"; and it routes and
prunes, selecting the most relevant content and "prun[ing] irrelevant branches" to
keep the context concise. The explicit goal is to adapt retrieval to the reasoning
context "while avoiding combinatorial explosion caused by unconstrained expansion."
(arXiv:2606.06036.) The released code implements this as a tool-calling reasoning loop
over seven specialized query tools.

Assessment: the elegant part is the feedback. Because the model reasons between graph
hops, what it learns at hop two changes which edge it follows at hop three. That is the
"reconstruction" that a fixed top-K retrieval cannot do, and it is what lets MRAgent
reach evidence a passive lookup would miss.

## The efficiency numbers

FACT: on LongMemEval, per query, MRAgent used about **118,000 tokens** at 586 seconds.
The compared systems used far more: A-Mem about 632,000 tokens, MemoryOS about 273,000,
and **LangMem about 3,268,000** (roughly 3.26 million). MRAgent versus LangMem is
therefore about a **27-fold reduction in tokens**, the headline "118K versus 3.26M"
result. Runtime was roughly half that of A-Mem (586 versus 1,122 seconds), though Mem0
was slightly faster in absolute terms. (arXiv:2606.06036, efficiency table.)

FACT: the benchmarks were LoCoMo and LongMemEval; the backbone models were
Gemini-2.5-Flash and Claude-Sonnet-4.5; and the baselines were RAG, A-Mem, MemoryOS,
LangMem, and Mem0. The paper reports accuracy "improvements over strong baselines (up
to 23%)." (arXiv:2606.06036.) Assessment: the specific per-backbone scores that
circulate in summaries (for example a Gemini judge-score rising from 68.31 to 84.21)
are from secondary summaries rather than verified in the paper text; treat the
"up to 23%" headline as the reliable figure and the exact split as uncertain.

## The limits

FACT: the paper is candid about two limitations. First, **latency scales with
exploration depth**: "because relational reasoning is deferred to retrieval, the cost
of reconstruction grows with the depth of exploration, and queries that require many
traversal steps incur higher latency than single-shot retrieval." Second, **the memory
is static and grows without bound**: "our static construction does not update or
consolidate memory over time, so the memory graph grows monotonically as interactions
accumulate, raising storage overhead in long-lived deployments." Future work named is
adaptive construction, lightweight memory maintenance, and more robust traversal
policies. (arXiv:2606.06036, Section 7.)

Assessment: this places MRAgent neatly against the landscape in
[chapter 3](03-memory-for-agents). Where Mem0 and LangMem put their intelligence into
*writing* memory (extract and consolidate as you go) and Zep puts it into a *temporal*
graph that closes out stale facts, MRAgent puts its intelligence into *reading*, a
reasoning-driven traversal at query time. Its static, ever-growing store is the price
of that choice; it spends cheaply per query but does not yet forget. The
token-efficiency result is the genuinely striking part: doing more reasoning at read
time can cost dramatically *fewer* tokens than the alternatives, because it avoids
hauling large fixed retrievals into the context window.

## Sources

- *Memory is Reconstructed, Not Retrieved: Graph Memory for LLM Agents* (arXiv:2606.06036) — https://arxiv.org/abs/2606.06036 (full text: https://arxiv.org/html/2606.06036v1)
- MRAgent code repository — https://github.com/Ji-shuo/MRAgent
- OpenReview record — https://openreview.net/forum?id=YPoHy6lgKP
