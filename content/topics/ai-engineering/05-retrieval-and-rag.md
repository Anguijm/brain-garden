---
type: lesson
series: ai-engineering
chapter: 5
title: Retrieval and RAG
status: curated
tags: [ai, ai-agents, llm, agent-engineering, rag, retrieval]
created: 2026-06-28
---

# Chapter 5 — Retrieval and RAG

Retrieval-augmented generation (RAG) is how you give a model facts it was not trained
on: you store your documents, find the relevant pieces at question time, and put them
in the context window before the model answers. This chapter walks the pipeline,
names the knobs that matter most, lists the usual ways it breaks, and gives a decision
rule for when to use RAG versus the alternatives.

## The pipeline

Assessment: the canonical RAG pipeline has two halves. You build an index once
(ingest documents, split them into chunks, turn each chunk into an embedding vector,
and store those in a vector index). Then, for every query, you retrieve the top
matching chunks, optionally rerank them, build the context, and generate the answer.
(This is common-practice consensus, matching the Databricks and StackAI guides.)

![Diagram: phase one indexes documents (documents, chunk, embed, index); phase two answers a query (retrieve top-K, rerank, build context, generate); the index feeds retrieval](img/rag-pipeline.png)
*The RAG pipeline, in two phases. Diagram.*

## The knobs that matter

Assessment: a few stages carry most of the quality.

- **Chunking** is the highest-leverage knob. A common production baseline is roughly
  512 to 1024 tokens per chunk with about 20 to 25 percent overlap, so you do not
  split a definition or a procedure across a boundary. Chunking sets the trade-off
  between recall (does any chunk contain the answer) and precision (is the chunk tight
  enough to be useful). Strategies range from fixed-size to recursive, semantic, and
  document-structure-aware.
- **Embeddings** turn text into vectors. FACT: they are benchmarked by MTEB (the
  Massive Text Embedding Benchmark) across many task categories; strong open models
  score in the high sixties of a percent. (MTEB reporting.) Assessment: pick an
  embedding model that suits your domain, a generic model can underperform on
  specialized vocabulary.
- **Retrieval** fetches the top K chunks (often 5 to 10). You improve it with hybrid
  search (dense vectors plus sparse keyword search such as BM25), metadata filters,
  and similarity thresholds; hybrid is commonly cited as adding single-digit
  percentage points of recall over pure vector search.
- **Reranking** with a cross-encoder, which scores the query and a passage together,
  is the standard precision boost after first-stage retrieval.

## How RAG breaks

Assessment: the common failure modes, and their fixes:

- **Bad chunking.** Chunks too big drag in noise and dilute the embedding; chunks too
  small split the answer across pieces so retrieval misses it.
- **Retrieval misses.** Usually a vocabulary mismatch between the query and the
  corpus, a weak embedding model for the domain, or too small a K. Fixes: hybrid
  search, query rewriting or expansion, and reranking.
- **Stale indexes.** If the source documents change and you do not re-embed, or you
  carry no freshness metadata, you get confident, wrong answers.
- **Lost in the middle at generation time.** Stuffing many retrieved chunks
  reintroduces context rot (see [chapter 4](04-context-engineering)); reranking and a
  tight K matter.
- **No grounding.** Without enforced citations, the model can hallucinate a synthesis
  on top of correct chunks.

## RAG versus the alternatives

The most common real question is not "how do I do RAG" but "do I even need it?"

Assessment (community consensus across 2025 and 2026 write-ups; treat these as
judgment, not fact):

- **Use RAG** for knowledge that is large, changes often, or must be cited and
  audited: document question-answering, support, policy lookup, regulated workflows.
  RAG fixes "missing facts."
- **Use fine-tuning** for *behavior*: tone, structured-output format, classification,
  tool-use policy, domain style. Fine-tuning fixes "wrong behavior," not stale facts.
  The rule of thumb is "fine-tune when behavior is the bottleneck, not missing facts."
- **Use long context** (just put the whole document in the window) for prototyping or
  one-off whole-document analysis. It is cited as roughly 20 times more expensive than
  RAG at scale and is still subject to ranking and context-rot problems, so it is not
  a substitute for retrieval discipline.
- **Use agent memory** (Mem0, Letta, LangMem; see [chapter 3](03-memory-for-agents))
  for evolving, per-user state across sessions, preferences, history, learned
  procedures, rather than a static corpus.

Assessment: hybrids dominate production. The decision is rarely either-or, you commonly
combine RAG for facts with light fine-tuning for behavior, and agent memory for
per-user state on top.

## Sources

- Databricks, *The Ultimate Guide to Chunking Strategies for RAG* — https://community.databricks.com/t5/technical-blog/the-ultimate-guide-to-chunking-strategies-for-rag-applications/ba-p/113089
- StackAI, *RAG Best Practices for Enterprise AI* — https://www.stackai.com/insights/retrieval-augmented-generation-(rag)-best-practices-for-enterprise-ai-chunking-embeddings-reranking-and-hybrid-search-optimization
- MTEB (Massive Text Embedding Benchmark) — https://huggingface.co/spaces/mteb/leaderboard
