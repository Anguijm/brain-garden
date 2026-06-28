---
type: lesson
series: ai-engineering
chapter: 0
title: AI agent engineering — overview
status: curated
tags: [ai, ai-agents, llm, agent-engineering, reference]
created: 2026-06-28
---

# AI agent engineering: a working reference

This wing is a plain-English reference on how to build software around large
language models (LLMs): the workflows and agents that wrap a model, the tools and
memory they use, the context you feed them, how you evaluate them, and how they fail.
It is written for someone who wants to understand the moving parts well enough to
build or judge a real system, not just use a chatbot. Every load-bearing claim is
labeled FACT (verified against a primary source), Assessment (my judgment), or
Speculation (forward-looking), per the vault's verification rules.

It ends with a dedicated deep dive on **MRAgent**, a 2026 research framework for
agent memory whose central idea is that memory should be *reconstructed*, not just
*retrieved*.

## The one picture to hold in your head

Assessment: almost everything here is "the model plus something." The model reasons,
decides, and writes. Around it you bolt on the ability to *act* (tools), to
*remember* (memory), to *see the right things right now* (context), and to *pull in
facts on demand* (retrieval). The rest of the discipline is how you compose, test,
and contain that.

![Diagram: a central LLM surrounded by Tools & MCP, Memory, Context, and Retrieval/RAG, with a bottom band of cross-cutting concerns: workflows vs agents, multi-agent systems, evaluation, and safety](img/landscape.png)
*What the chapters in this wing cover. Diagram.*

## The chapters

The wing reads in order, but each chapter stands alone.

1. **[Workflows vs agents](01-workflows-vs-agents)** — the core distinction, the
   "augmented LLM" building block, the five workflow patterns, and the agent loop.
   The most important chapter; start here.
2. **[Tools and MCP](02-tools-and-mcp)** — how a model acts on the world: tool use
   (function calling), designing good tools, and the Model Context Protocol that
   standardizes the wiring.
3. **[Memory for agents](03-memory-for-agents)** — short-term vs long-term;
   semantic, episodic, and procedural memory; how memory is written and read; and
   the current framework landscape.
4. **[Context engineering](04-context-engineering)** — managing the context window
   as a finite resource: context rot, compaction, note-taking, and sub-agents.
5. **[Retrieval and RAG](05-retrieval-and-rag)** — the retrieval-augmented
   generation pipeline, and when to reach for RAG versus fine-tuning, long context,
   or agent memory.
6. **[Evaluation and testing](06-evaluation-and-testing)** — how you know it works:
   golden datasets, LLM-as-judge (and its biases), and trajectory versus end-state
   scoring.
7. **[Multi-agent systems](07-multi-agent-systems)** — orchestrator-and-workers, when
   multiple agents genuinely help, when they just burn tokens, and how errors compound.
8. **[Safety and best practices](08-safety-and-best-practices)** — the OWASP Top 10
   for LLM applications, excessive agency, guardrails, and a consolidated checklist.
9. **[MRAgent: reconstructive memory](09-mragent)** — the dedicated deep dive: the
   Cue-Tag-Content graph, reconstruct-not-retrieve, and the efficiency numbers.
10. **[Glossary](glossary)** — the terms in one place.

## The single most useful principle

FACT: Anthropic's guidance for building agents is to "find the simplest solution
possible, and only increase complexity when needed," and to "add complexity only
when it demonstrably improves outcomes." (Anthropic, *Building Effective Agents*.)

Assessment: this is the thread running through the whole wing. A single well-prompted
model call beats a workflow you do not need; a workflow beats an agent you cannot
evaluate; one agent beats five that mostly talk to each other. Agency (letting the
model decide its own steps) is a cost you pay in latency, tokens, and debuggability,
not a feature you add for its own sake. Reach for it only when the steps genuinely
cannot be predicted in advance.
