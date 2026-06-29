---
type: lesson
series: ai-engineering
chapter: 0
title: AI agents, in plain English — overview
status: curated
tags: [ai, ai-agents, llm, agent-engineering, reference, beginner-friendly]
created: 2026-06-28
---

# AI agents, in plain English

This section explains how people build useful tools on top of AI models, one piece at a
time, in plain language. You do not need a computer background to follow it. If the whole
idea is new to you, begin with the first basics chapter on what a model actually is. It
explains the handful of words the rest of the section leans on.

A note on trust before we start. AI can sound completely sure and still be wrong, so every
claim here carries a label. FACT means I checked it against a reliable source. Assessment
means it is my own judgment. Speculation means it is a guess about where things are headed.

## The one picture to keep in mind

Almost everything in this section is "the model, plus help." On its own, a model reads and
writes. To make it genuinely useful, people add a few abilities around it:

- **Tools** let it do things, such as searching the web or running code, instead of only
  writing.
- **Memory** lets it remember earlier chats.
- **Context** is whatever you put in front of it right now, and the skill is choosing the
  right things to include.
- **Retrieval** lets it look up facts instead of guessing from memory.

Everything else is about combining those pieces, testing them, and keeping them safe.

![Diagram: a central AI model surrounded by Tools, Memory, Context, and Retrieval, with a bottom row of bigger topics: workflows vs agents, multi-agent systems, testing, and safety](img/landscape.png)
*What this section covers. Diagram.*

## The chapters

Read them in order if you are learning, or jump to what you need. Each one stands on its
own.

**Start with the basics**

- **[What is an AI model?](01-what-is-an-llm)** — the ground floor: model, token, prompt,
  and the three big limits to keep in mind.
- **[How to write a good prompt](02-how-to-prompt)** — the easiest way to get better
  answers, with no technical skill needed.
- **["Thinking" and reasoning models](03-reasoning-models)** — what it means when a model
  works through a problem before answering, and when that is worth it.
- **[Cost and speed](04-cost-and-speed)** — how you pay (by the token), and why long
  inputs cost more and run slower.
- **[Pictures and voice](05-pictures-and-voice)** — models that can also read images and
  handle sound, and where they fall short.

**Then, building with models**

- **[Workflows vs agents](06-workflows-vs-agents)** — the most important idea: how much
  freedom to give the model, and the common patterns.
- **[Tools and MCP](07-tools-and-mcp)** — how a model does things in the real world, and
  the standard "plug" for hooking tools up.
- **[Memory](08-memory-for-agents)** — how a model remembers across chats.
- **[Context](09-context-engineering)** — the model can only read so much at once; this is
  how you keep what it reads short and useful.
- **[Retrieval and RAG](10-retrieval-and-rag)** — feeding a model the right facts so it
  does not have to guess.
- **[Testing](11-evaluation-and-testing)** — how you know it actually works.
- **[Many agents at once](12-multi-agent-systems)** — when several agents help, and when
  they just waste money.
- **[Safety and good habits](13-safety-and-best-practices)** — the main ways these systems
  go wrong, and a plain checklist.
- **[MRAgent: a closer look](14-mragent)** — a 2026 research idea for AI memory, with an
  honest read on what holds up.

**Reference**

- **[Word list](glossary)** — the terms in one place.

## The most useful rule

FACT: the best-known advice for building these systems is to "find the simplest solution
possible, and only increase complexity when needed." (Anthropic, *Building Effective
Agents*.)

Assessment: this rule runs through the whole section. A plain model call beats a fancy
setup you do not need. One agent beats five that mostly talk to each other. Giving the
model more freedom costs more money, more waiting, and makes problems harder to find. So
add freedom only when you truly need it.
