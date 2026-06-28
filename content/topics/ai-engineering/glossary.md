---
type: lesson
series: ai-engineering
chapter: 10
title: AI agent engineering — glossary
status: curated
tags: [ai, ai-agents, llm, agent-engineering, glossary]
created: 2026-06-28
---

# Glossary

The terms used across this wing, in one place. Each points to the chapter that
develops it.

**Agent.** A system where an LLM "dynamically directs its own processes and tool
usage," deciding the order of steps at runtime. Contrast with a workflow.
([Chapter 1](01-workflows-vs-agents).)

**Agent loop.** The cycle an agent runs: reason or plan, call tools, observe the
results from the environment, repeat until a stopping condition. ([Chapter 1](01-workflows-vs-agents).)

**Agent-Computer Interface (ACI).** The design of the tools an agent uses, the agent's
equivalent of a user interface; worth as much design effort. ([Chapter 2](02-tools-and-mcp).)

**Augmented LLM.** The base building block: a model enhanced with retrieval, tools,
and memory, which it uses actively. ([Chapter 1](01-workflows-vs-agents).)

**Compaction.** Summarizing a nearly-full context window and starting a fresh window
from the summary, to keep going without losing the thread. ([Chapter 4](04-context-engineering).)

**Context engineering.** Curating and maintaining the optimal set of tokens in the
model's window across many turns; the successor to prompt engineering. ([Chapter 4](04-context-engineering).)

**Context rot.** The degradation in a model's recall as the number of tokens in its
context window grows. ([Chapter 4](04-context-engineering).)

**Cue-Tag-Content graph.** MRAgent's memory structure: fine-grained cues linked through
associative tags to memory-content items, as triplets. ([Chapter 9](09-mragent).)

**Embedding.** A vector representation of text that places similar meanings near each
other, the basis of vector search in RAG. ([Chapter 5](05-retrieval-and-rag).)

**Episodic memory.** Memory of specific past events or interactions, often replayed as
few-shot examples. "What happened." ([Chapter 3](03-memory-for-agents).)

**Evaluator-optimizer.** A workflow where one LLM call generates and another evaluates
in a loop; also the canonical guardrail-by-loop. ([Chapter 1](01-workflows-vs-agents),
[chapter 8](08-safety-and-best-practices).)

**Excessive agency (LLM06).** The risk that an agent has more functionality,
permissions, or autonomy than it safely needs. ([Chapter 8](08-safety-and-best-practices).)

**Function calling.** See tool use. ([Chapter 2](02-tools-and-mcp).)

**Golden dataset.** A set of ideal interactions (trajectory plus final answer) used as
ground truth for evaluation. ([Chapter 6](06-evaluation-and-testing).)

**Grounding.** Tying a model's answer to retrieved source material, usually with
enforced citations, to reduce hallucination. ([Chapter 5](05-retrieval-and-rag),
[chapter 8](08-safety-and-best-practices).)

**Hybrid search.** Combining dense vector search with sparse keyword search (such as
BM25) to improve retrieval recall. ([Chapter 5](05-retrieval-and-rag).)

**LLM-as-judge.** Using a model to grade output against a rubric; powerful but subject
to position, verbosity, and self-preference bias. ([Chapter 6](06-evaluation-and-testing).)

**MCP (Model Context Protocol).** An open standard for connecting AI applications to
external tools and data, "a USB-C port for AI applications." ([Chapter 2](02-tools-and-mcp).)

**MRAgent.** "Reconstructive Memory Agent," a 2026 framework whose memory is
reconstructed at query time by reasoning over an associative graph rather than
retrieved by fixed lookup. ([Chapter 9](09-mragent).)

**Orchestrator-workers.** A pattern where a lead LLM dynamically breaks a task into
subtasks (not predefined), delegates to workers, and synthesizes the results.
([Chapter 1](01-workflows-vs-agents), [chapter 7](07-multi-agent-systems).)

**Procedural memory.** Memory of the rules and skills for performing tasks, often held
in the prompt. "How you do it." ([Chapter 3](03-memory-for-agents).)

**Prompt chaining.** A workflow that splits a task into a fixed sequence of LLM calls,
each consuming the previous one's output. ([Chapter 1](01-workflows-vs-agents).)

**Prompt injection (LLM01).** An attack that manipulates input to override the model's
instructions; *direct* (in the user prompt) or *indirect* (hidden in ingested
content). ([Chapter 8](08-safety-and-best-practices).)

**RAG (retrieval-augmented generation).** Giving a model facts at question time by
retrieving relevant stored chunks into its context before it answers. ([Chapter 5](05-retrieval-and-rag).)

**Reranking.** A second-stage scoring of retrieved passages (often with a
cross-encoder) to raise precision before generation. ([Chapter 5](05-retrieval-and-rag).)

**Routing.** A workflow that classifies an input and sends it to a specialized
follow-up. ([Chapter 1](01-workflows-vs-agents).)

**Semantic memory.** Memory of facts and concepts about the user or world. "What you
know." ([Chapter 3](03-memory-for-agents).)

**Stopping condition.** A rule that ends the agent loop: task complete, a maximum
number of steps, or a human checkpoint. ([Chapter 1](01-workflows-vs-agents).)

**Structured note-taking.** Writing notes to storage outside the context window and
pulling them back when needed; a scratchpad. ([Chapter 4](04-context-engineering).)

**Tool use.** The mechanism by which a model calls functions you define: it emits a
`tool_use` request, your code returns a `tool_result`, and the exchange repeats. Also
called function calling. ([Chapter 2](02-tools-and-mcp).)

**Trajectory evaluation.** Scoring the path an agent took (its sequence of tool calls
and reasoning), not just the final answer. Contrast with end-state evaluation.
([Chapter 6](06-evaluation-and-testing).)

**Unbounded consumption (LLM10).** The risk of runaway resource use and cost,
including "denial of wallet." ([Chapter 8](08-safety-and-best-practices).)

**Workflow.** A system where LLMs and tools are orchestrated through predefined code
paths you wrote in advance. Contrast with an agent. ([Chapter 1](01-workflows-vs-agents).)
