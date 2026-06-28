---
type: lesson
series: ai-engineering
chapter: 1
title: Workflows vs agents
status: curated
tags: [ai, ai-agents, llm, agent-engineering, workflows]
created: 2026-06-28
---

# Chapter 1 — Workflows vs agents

The first choice you make is simple to say: how much freedom do you give the model? You
can keep it on a track you built, or you can let it find its own way. This chapter
explains both, and shows the common shapes most real systems use.

## Two kinds: workflow and agent

There are two main ways to build with a model.

FACT: in a **workflow**, the model and its tools follow steps you wrote ahead of time,
in code. In an **agent**, the model decides its own steps as it goes. (Anthropic,
*Building Effective Agents*; the same split shows up in the LangGraph docs.)

The real difference is who picks the order of steps. In a workflow, you do, before it
ever runs. In an agent, the model does, while it runs, based on what it finds.

![Diagram: a line from a single model call, to a workflow (set steps you wrote), to an agent (the model picks its own steps), with a note that more freedom means more cost](img/spectrum.png)
*From one call to an agent: more freedom means more cost. Diagram.*

## Start simple

This is the most important habit in the whole section.

FACT: the standard advice is to "find the simplest solution possible, and only increase
complexity when needed." Workflows are steady and easy to predict for clear tasks.
Agents are better when the work needs flexibility, but they cost more and run slower.
(Anthropic.)

Assessment: a good default is a single model call, maybe with a search step and a couple
of examples. Move up to a workflow only when the task breaks into a few set steps. Move
up to an agent only when you truly cannot know the steps ahead of time. Each step up
buys flexibility and costs you money, speed, and the ability to test the thing.

FACT: ready-made frameworks (such as LangGraph or Rivet) make it easy to start, but they
add extra layers that hide what the model is really doing, which makes problems harder to
find. The advice is to start with the plain model and add a framework only once you
understand what is underneath. (Anthropic.)

## The building block: the model plus a few add-ons

Every workflow and agent is built from the same base part: a model with a few add-ons.

FACT: that base part is a model that can do three extra things: **search** (it writes its
own searches), **use tools** (it picks and runs them), and **keep notes** (it decides
what to hold on to). (Anthropic.) The rest of this section is really just a closer look at
those three.

## Five common workflow patterns

FACT: most workflows look like one of five shapes. These come from Anthropic and match
what LangGraph and OpenAI describe, so they are widely agreed on, not one company's
style.

1. **Chain (do it in steps).** Break the task into a fixed line of steps. Each step takes
   the last step's output. You can add a quick check between steps. Good when a task
   splits cleanly into set parts.
2. **Route (sort, then send).** First sort the request into a type, then send it to the
   step built for that type. Good when requests fall into clear groups, like a refund
   question versus a tech question.
3. **Parallel (run at the same time).** Run several model calls at once and combine the
   results. Two uses: split a task into separate parts done side by side, or run the same
   task a few times and compare for a more confident answer.
4. **Manager and helpers.** A main "manager" model breaks the job into smaller jobs,
   hands them to helper models, and combines what they return. The difference from
   "parallel" is that the manager decides the smaller jobs on the spot, instead of you
   setting them in advance. Good for big, messy tasks, like changes spread across many
   files.
5. **Make and check.** One model writes a draft. A second model reviews it and gives
   notes. They loop until it is good. Good when you can clearly say what "good" means.
   It is the machine version of draft-and-revise.

Assessment: "route" and "chain" alone handle a surprising amount of real work. "Manager
and helpers" is the one that turns into a multi-agent system (see
[chapter 7](07-multi-agent-systems)); the only real question there is whether the helpers
are full agents.

## The agent loop

When you do give the model control, here is what it actually does: it runs a loop.

FACT: the short definition of an agent is a model "using tools in a loop." It starts from
your request, then plans and works on its own, and may come back to you for help. At each
turn it checks real results from the world (like what a tool returned) to see how it is
doing. (Anthropic, *Effective Context Engineering* and *Building Effective Agents*.)

![Diagram: a three-step cycle, plan, then use a tool, then look at the result, and back to plan; it repeats until the task is done or it is stopped](img/agent-loop.png)
*The agent loop. Diagram.*

FACT: the loop needs **rules for when to stop**, such as "the task is done," "you hit a
set number of tries," or "pause and ask a human." Without a stop rule, it can run too
long. (Anthropic.)

FACT: small mistakes can pile up over many turns, so agents need careful testing in a
safe, walled-off setup, plus safety limits. (Anthropic.) That is what
[chapter 6](06-evaluation-and-testing) and [chapter 8](08-safety-and-best-practices) are
about.

## Sources

- Anthropic, *Building Effective Agents* — https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, *Effective Context Engineering for AI Agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- LangChain / LangGraph, *Workflows and agents* — https://docs.langchain.com/oss/python/langgraph/workflows-agents
