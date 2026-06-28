---
type: lesson
series: ai-engineering
chapter: 3
title: Memory for agents
status: curated
tags: [ai, ai-agents, llm, agent-engineering, memory]
created: 2026-06-28
---

# Chapter 3 — Memory for agents

A model has no memory of its own between calls; everything it "knows" in a session is
whatever you put in front of it. Agent memory is the engineering that lets a system
remember facts, past interactions, and learned habits across turns and across sessions.
This chapter lays out the standard taxonomy, how memory gets written and read, and the
framework landscape. The next-generation research approach to memory gets its own
[chapter 9 on MRAgent](09-mragent).

## Short-term versus long-term

FACT: memory splits into short-term (also called working memory) and long-term. In
LangChain and LangGraph terms, short-term memory is "thread-scoped": it "tracks the
ongoing conversation by maintaining message history within a session." Long-term
memory is saved in named "namespaces," persists beyond a single thread, and can be
retrieved "at any time and in any thread." (LangChain docs.)

Assessment: in practice, short-term memory *is* the context window, the system
prompt, tool definitions, message history, and scratchpad. It is volatile and bounded
by the window. Long-term memory is external storage (a vector database, a key-value
store, files, or a graph) that gets read back into the window on demand. Managing the
short-term side well is its own discipline, covered in [chapter 4](04-context-engineering).

![Diagram: short-term working memory is the context window; long-term memory splits into semantic (what you know), episodic (what happened), and procedural (how you do it), with write paths and frameworks listed below](img/memory-taxonomy.png)
*The memory taxonomy for agents. Diagram.*

## The three kinds of long-term memory

FACT: the common split, borrowed explicitly from human cognitive psychology, is three
types (LangChain docs; LangMem):

- **Semantic memory** is "the retention of specific facts and concepts," used to
  personalize by remembering facts about the user or the world. It is often stored as
  a profile, a collection, or knowledge triples.
- **Episodic memory** is "recalling past events or actions," memories of specific
  past interactions. It is often implemented by storing successful past episodes and
  replaying them as few-shot examples.
- **Procedural memory** is "remembering the rules used to perform tasks,"
  generalized skills and behavior. It is held across model weights, agent code, and
  the prompt; LangMem focuses on saving learned procedures as updated instructions in
  the agent's prompt.

Assessment: the mnemonic is semantic = *what you know*, episodic = *what happened*,
procedural = *how you do it*.

## How memory gets written and read

The hard problem is not storing a fact, it is deciding when to write and how to keep
the store consistent over time.

FACT: LangChain names two write paths. Writing "in the hot path" forms memories during
runtime, so they are "immediately available" but they "impact agent latency." Writing
"in the background" uses a separate task, which "eliminates latency" but forces you to
decide how often to write and when to trigger formation. Reads use a namespace and key
and support semantic search and filtering. (LangChain docs.)

FACT: Mem0's write path runs two phases: an extraction phase where an LLM pulls salient
facts from the conversation, and an update phase that compares new information against
existing memories and issues `ADD`, `UPDATE`, `DELETE`, or `NOOP` operations to keep
the store consistent. (Mem0 paper, arXiv:2504.19413.)

Assessment: practical write triggers are the end of a session, crossing a token
budget, or an LLM judging something salient enough to keep. The recurring hard part is
*update and consolidation*, resolving contradictions, de-duplicating, and expiring
stale facts, not the initial store.

## The framework landscape

Assessment: the field clusters into three shapes. Read this as a map, not an
endorsement; the benchmark numbers below are self-reported by each system's authors
against baselines they chose, so treat them as claims, not settled fact.

**Operating-system-style tiered memory.** FACT: MemGPT (arXiv:2310.08560, "Towards
LLMs as Operating Systems") borrows from OS virtual memory. It pages information
between a fast in-window "main context," a searchable "recall storage" of conversation
history, and a long-term "archival storage" vector store, and the model edits its own
memory through function calls. Letta is the open-source framework MemGPT became, with
the same core, recall, and archival tiers.

**LLM-managed memory layers.** FACT: Mem0 is a "universal memory layer" that
extracts, consolidates, and retrieves facts and drops in over an existing agent; its
paper reports large latency and token-cost savings versus a full-context baseline on
the LOCOMO benchmark, and a graph variant (Mem0g) that scores slightly higher.
LangMem is LangChain's SDK for extracting and managing semantic, episodic, and
procedural memories over a pluggable store.

**Temporal knowledge graphs.** FACT: Zep, built on the open-source Graphiti engine,
is a "temporally-aware knowledge graph." It uses a bi-temporal model that tracks both
when an event occurred and when it was ingested; on a contradiction it closes the old
fact's validity window rather than deleting it, which keeps history queryable and
supports point-in-time questions like "what was the user's plan in January?" (Zep
paper, arXiv:2501.13956.)

Assessment: vector-only memory is the simplest but weakest at relational and temporal
reasoning. Graph and temporal systems add that capability at a higher ingestion cost.
Which you need is set by your queries: simple fact recall versus multi-hop "what
changed when" reasoning. The MRAgent work in [chapter 9](09-mragent) is a fourth
shape, a graph you actively *reconstruct* over rather than passively retrieve from.

## Sources

- LangChain, *Memory overview* — https://docs.langchain.com/oss/python/concepts/memory
- LangChain, *LangMem SDK launch* — https://www.langchain.com/blog/langmem-sdk-launch
- Mem0, *Building Production-Ready AI Agents with Scalable Long-Term Memory* (arXiv:2504.19413) — https://arxiv.org/abs/2504.19413
- MemGPT, *Towards LLMs as Operating Systems* (arXiv:2310.08560) — https://arxiv.org/abs/2310.08560
- Zep, *A Temporal Knowledge Graph Architecture for Agent Memory* (arXiv:2501.13956) — https://arxiv.org/abs/2501.13956
