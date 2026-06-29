---
type: lesson
series: ai-engineering
chapter: 15
title: AI agent engineering — word list
status: curated
tags: [ai, ai-agents, llm, agent-engineering, glossary]
created: 2026-06-28
---

# Word list

Every term used in this section, in plain language and in one place. Each entry points to
the chapter that explains it in full.

**Agent.** A system where the model decides its own steps as it runs, instead of following
steps you set in advance. The opposite of a workflow. (See
[Workflows vs agents](06-workflows-vs-agents).)

**Agent loop.** The cycle an agent repeats: plan, use a tool, look at the result, and plan
again, until it is done or it is stopped. (See [Workflows vs agents](06-workflows-vs-agents).)

**API.** An agreed way for one program to ask another program to do something. It is how a
model's tools reach the outside world. (See [Tools and MCP](07-tools-and-mcp).)

**Compaction.** Summarizing a nearly-full context window and starting a fresh one from the
summary, so the work can continue. (See [Context](09-context-engineering).)

**Context engineering.** The skill of choosing what to put into the model's window on each
turn, and what to leave out. (See [Context](09-context-engineering).)

**Context rot.** The way a model gets less reliable at recalling any one detail as the
window fills up with more text. (See [Context](09-context-engineering).)

**Context window.** How much text a model can read at one time: your prompt plus everything
said in the chat so far. (See [What is an AI model?](01-what-is-an-llm).)

**Cue-Tag-Content graph.** MRAgent's memory: a web of small keywords (cues) linked through
middle points (tags) to actual saved memories (content). (See [MRAgent](14-mragent).)

**Embedding.** A list of numbers that captures the meaning of a piece of text, so a search
can find items by meaning rather than by exact words. (See
[Retrieval and RAG](10-retrieval-and-rag).)

**Episodic memory.** Memory of past events: specific things that happened before, often
replayed later as examples. "What happened." (See [Memory](08-memory-for-agents).)

**Evaluator-optimizer (make and check).** A setup where one model writes a draft and a
second model reviews it, looping until it is good. (See
[Workflows vs agents](06-workflows-vs-agents) and [Safety](13-safety-and-best-practices).)

**Excessive agency.** The risk of giving an agent more power, permission, or freedom than
it safely needs. (See [Safety](13-safety-and-best-practices).)

**Fine-tuning.** Training a model a little further on your own examples so it picks up a
tone, a format, or a way of working. It changes behavior, not facts. (See
[Retrieval and RAG](10-retrieval-and-rag).)

**Function calling.** Another name for tool use. (See [Tools and MCP](07-tools-and-mcp).)

**Golden dataset.** A set of example questions, each paired with the ideal answer, used as
the standard you grade against. (See [Testing](11-evaluation-and-testing).)

**Grounding.** Tying a model's answer to real source material, usually by making it cite
its sources, to cut down on made-up claims. (See [Retrieval and RAG](10-retrieval-and-rag)
and [Safety](13-safety-and-best-practices).)

**Hallucination.** When a model produces something false but believable, stated with the
same confidence as a correct answer. (See [What is an AI model?](01-what-is-an-llm).)

**Hybrid search.** Finding documents by both meaning and exact keywords, which catches
names and codes that meaning alone can miss. (See [Retrieval and RAG](10-retrieval-and-rag).)

**Latency.** The delay before you get an answer, the wait time. (See
[Cost and speed](04-cost-and-speed).)

**Least privilege.** A security rule: give a system only the minimum power and access it
needs, and nothing more. (See [Safety](13-safety-and-best-practices).)

**LLM (large language model).** A computer program that works with words by guessing the
next word, over and over. (See [What is an AI model?](01-what-is-an-llm).)

**LLM-as-judge.** Using a model to grade another model's answers against a checklist.
Powerful, but the grader has its own biases. (See [Testing](11-evaluation-and-testing).)

**MCP (Model Context Protocol).** An open standard for connecting AI apps to outside tools
and data, "a USB-C port for AI applications." (See [Tools and MCP](07-tools-and-mcp).)

**Multimodal.** A model that can handle more than one kind of input, such as images or
sound, not just text. (See [Pictures and voice](05-pictures-and-voice).)

**MRAgent.** A 2026 research idea for AI memory that the model rebuilds at question time by
reasoning over a web of linked notes, rather than just looking it up. (See
[MRAgent](14-mragent).)

**Orchestrator-workers (manager and helpers).** A lead model that breaks a job into smaller
jobs, hands them to helper models, and combines the results. (See
[Workflows vs agents](06-workflows-vs-agents) and
[Many agents at once](12-multi-agent-systems).)

**Procedural memory.** Memory of know-how: the rules and habits for doing a task, often
kept in the prompt. "How you do it." (See [Memory](08-memory-for-agents).)

**Prompt.** Whatever you type to the model: your request plus any background you give it.
(See [How to write a good prompt](02-how-to-prompt).)

**Prompt injection.** An attack that slips hidden instructions into what the model reads, in
order to hijack it. (See [Safety](13-safety-and-best-practices).)

**RAG (retrieval-augmented generation).** Giving a model the facts at question time by
finding relevant saved text and placing it in the window before the model answers. (See
[Retrieval and RAG](10-retrieval-and-rag).)

**Reasoning model.** A model that writes out a private chain of steps before it answers,
which helps on hard problems but costs more and runs slower. (See
[Reasoning models](03-reasoning-models).)

**Reranking.** A second, more careful pass that re-sorts search results so the best ones
land on top before they reach the model. (See [Retrieval and RAG](10-retrieval-and-rag).)

**Routing.** A workflow that sorts a request into a type and sends it to the step built for
that type. (See [Workflows vs agents](06-workflows-vs-agents).)

**Semantic memory.** Memory of facts: things the system knows about you or the world. "What
you know." (See [Memory](08-memory-for-agents).)

**Stopping condition.** A rule that ends the agent loop: the task is done, a step limit is
reached, or it pauses for a human. (See [Workflows vs agents](06-workflows-vs-agents).)

**Structured note-taking.** Writing notes to storage outside the window and pulling them
back when needed, a scratchpad the model can return to. (See [Context](09-context-engineering).)

**Token.** The small chunk a model reads and writes in, roughly three-quarters of a word.
You pay per token, and there is a limit on how many fit at once. (See
[What is an AI model?](01-what-is-an-llm).)

**Tool use.** How a model triggers an outside action: it asks for a tool by name, your code
runs it and returns the result, and the exchange repeats. Also called function calling.
(See [Tools and MCP](07-tools-and-mcp).)

**Training.** Showing a model a huge amount of text ahead of time so it slowly gets better
at predicting text. (See [What is an AI model?](01-what-is-an-llm).)

**Trajectory (path) evaluation.** Grading the path an agent took, the steps and tools it
used, not just whether the final answer was right. (See [Testing](11-evaluation-and-testing).)

**Unbounded consumption.** The risk of an agent burning runaway amounts of money or
resources, sometimes called a "denial of wallet" attack. (See
[Safety](13-safety-and-best-practices).)

**Workflow.** A system where the model and its tools follow steps you wrote ahead of time.
The opposite of an agent. (See [Workflows vs agents](06-workflows-vs-agents).)
