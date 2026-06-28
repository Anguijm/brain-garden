---
type: lesson
series: ai-engineering
chapter: 7
title: Multi-agent systems
status: curated
tags: [ai, ai-agents, llm, agent-engineering, multi-agent]
created: 2026-06-28
---

# Chapter 7 — Multi-agent systems

A multi-agent system is several agents working on one problem, usually a lead agent
that delegates to specialized workers. It is the most powerful and the most expensive
pattern in this wing. This chapter covers the main architecture, the cases where extra
agents genuinely help, the cases where they just burn tokens, and the compounding-error
math that explains why long chains fail.

## The dominant pattern: orchestrator and workers

FACT: the production pattern Anthropic describes is the orchestrator-workers shape,
"a lead agent coordinates the process while delegating to specialized subagents that
operate in parallel," typically spinning up three to five subagents. Its key property,
distinguishing it from simple parallelization, is that "subtasks aren't pre-defined,
but determined by the orchestrator based on the specific input." (Anthropic, *Building
Effective Agents* and *How we built our multi-agent research system*.)

![Diagram: a lead agent at top breaks down the task, delegates, and synthesizes; three subagents below each with their own clean context; a "wins" box (breadth-first search, +90% over single-agent) and a "costs" box (~15x the tokens of a chat, errors compound)](img/multi-agent.png)
*Orchestrator and workers: the gains and the costs. Diagram.*

## When multiple agents genuinely help

FACT: multi-agent systems succeed on "breadth-first queries that involve pursuing
multiple independent directions simultaneously," where the information exceeds a single
context window. In Anthropic's evaluations, "a multi-agent system with Claude Opus 4 as
the lead agent and Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by
90.2%," and parallelization "cut research time by up to 90% for complex queries."
(Anthropic.)

Assessment: the real wins come from two things, parallelizable breadth (independent
directions explored at once) and separation of concerns (each subagent gets its own
clean context window and one focused objective, which is the context-engineering point
from [chapter 4](04-context-engineering)).

## When they just burn tokens

FACT: the cost is steep. "Agents typically use about 4 times more tokens than chat
interactions, and multi-agent systems use about 15 times more tokens than chats," and
"token usage by itself explains 80% of the variance" in quality. So multi-agent is
only economical for high-value tasks. (Anthropic.)

FACT: it is a poor fit when tasks need shared context or have many dependencies;
"most coding tasks involve fewer truly parallelizable tasks than research."
(Anthropic.) Observed coordination failures include an orchestrator "spawning 50
subagents for simple queries," agents "distracting each other with excessive updates,"
and agents getting stuck hunting for sources that do not exist. (Anthropic.)

## Why long chains fail: compounding errors

FACT: errors compound. "One step failing can cause agents to explore entirely
different trajectories, leading to unpredictable outcomes." (Anthropic.) The math: for
n sequential steps each succeeding with probability p, end-to-end success is p to the
n-th power, multiplied, not averaged. At 99% per step you are at 90.4% after 10 steps
and 36.6% after 100; at 95% per step you are at about 59% after 10 steps. Correlated
errors make real chains worse than the formula. (Compounding-error analyses.)

Assessment: this is the quantitative case for keeping agents short, adding checkpoints,
and preferring an end-state you can verify over a long unverified chain.

## Design lessons that hold up

FACT: Anthropic's reported lessons:

- Delegation must give each subagent "an objective, an output format, guidance on the
  tools and sources to use, and clear task boundaries." Vague delegation is the fast
  path to wasted work.
- Embed effort-scaling rules in the orchestrator's prompt: "Simple fact-finding
  requires just 1 agent with 3-10 tool calls, direct comparisons might need 2-4
  subagents with 10-15 calls each."
- A current bottleneck is that "lead agents execute subagents synchronously, waiting
  for each set of subagents to complete before proceeding."

Assessment: the hidden tax in multi-agent systems is coordination, the orchestrator
has to write good task boundaries and merge the results well. Misallocated subtasks or
fuzzy delegation degrade quality faster than just using a single agent would have. The
honest default, consistent with [chapter 1](01-workflows-vs-agents), is to reach for
multiple agents only when the work is genuinely broad and parallel and the value
justifies the roughly fifteen-fold token cost.

## Sources

- Anthropic, *How we built our multi-agent research system* — https://www.anthropic.com/engineering/multi-agent-research-system
- Anthropic, *Building Effective Agents* — https://www.anthropic.com/engineering/building-effective-agents
- *The math behind why multi-step AI agents fail in production* — https://medium.com/k8slens/the-math-behind-why-multi-step-ai-agents-fail-in-production-c6d60ea6ca31
