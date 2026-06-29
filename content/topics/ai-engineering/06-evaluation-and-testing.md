---
type: lesson
series: ai-engineering
chapter: 6
title: Evaluation and testing
status: curated
tags: [ai, ai-agents, llm, agent-engineering, evaluation, verification, using-ai-well, feedback-loops]
created: 2026-06-28
---

# Testing

The whole section so far has pushed one habit: do not simply trust what a model produces.
Testing is how you act on that, and builders shorten the word to "evaluation," or "evals."
Good testing tells you whether a change made things better or worse, and whether the
system is reliable enough to put in front of real users.

One thing makes this harder than ordinary software testing: models are not exact, so the
same input can produce a different answer each time, a property described as being
"stochastic." Because of that, a single passing test means little, and you have to measure
across many runs before you can trust the pattern you are seeing.

## Two places to test: before and during

Assessment: testing happens in two settings.

*Before you ship (offline testing)* runs the system against a fixed set of saved examples,
the same set every time, so you can tell whether a change helped or hurt. This is mainly
how you catch a "regression," which is when a change quietly breaks something that used to
work.

*While it runs (online testing)* watches real use in production, sampling real questions
and user feedback, to catch problems your saved examples never imagined.

FACT: even with solid automated tests, a person still has to spot-check by hand. Anthropic
reports that people testing agents catch edge cases the automated checks miss, including
confident, made-up answers on unusual questions. (Anthropic, *How we built our multi-agent
research system*.)

## The golden set

FACT: the heart of offline testing is a "golden dataset," a set of example questions each
paired with the ideal answer, and for an agent, the ideal sequence of steps. It is the
standard you grade everything against. (Google.)

FACT: you do not need a big one to start. Anthropic found that about 20 well-chosen
questions reflecting real use were enough to see whether a change helped. (Anthropic.)
Assessment: begin with a few dozen real examples, then grow the set from the failures you
find in production. Because answers vary from run to run, score across several runs, not a
single one.

## Letting a model grade the answers

Grading open-ended writing by hand does not scale, so the common fix is to have a second
model grade the first one's answers automatically.

FACT: this is called "LLM-as-judge." You give a grading model a clear checklist, called a
"rubric," for example: are the facts right, are the sources right, is the answer complete?
Anthropic found that a single grading model with a good checklist matched their own human
judgment. (Anthropic.) OpenAI describes the same two-step setup: one model answers, and a
stronger model grades it. (OpenAI.)

FACT: but the grader has its own blind spots, well documented in research. It tends to
favor whichever answer it sees first or last, regardless of quality. It leans toward its
own writing style, toward longer answers, and toward agreeing too easily. (*Judging the
Judges*, arXiv:2406.07791.) You can reduce these by swapping the order and averaging,
using several graders, and keeping the checklist short and clear. (CalibraEval,
arXiv:2410.15393.)

Assessment: the rule that ties this whole section together, do not blindly trust the AI,
applies to the grader too. Check the grader against human scores on a sample before you
rely on it, and keep spot-checking by hand. This is the same caution as the
[using-AI-well thread](../../connections/using-ai-well).

## Score the path, not just the answer

A right answer reached in a dangerous or wasteful way is still a failure, so you grade two
separate things.

![Diagram: a string of steps from start through several tool calls to an answer; one bracket grades the whole path, another grades only the final answer](img/eval.png)
*Grade where the agent ended up, and grade how it got there. Diagram.*

FACT: end-state grading asks "is the final answer right?" Path grading (the technical name
is trajectory grading) asks "was the route to it sound?", looking at the steps the agent
took, the tools it used, and the dead ends it hit. An agent that lands on the right answer
through a reckless or wildly inefficient path is still a problem in production. (Confident
AI; LangChain.) Anthropic mostly grades the end state, because agents reach the same goal
in many valid ways, but it still watches the path for safety. (Anthropic.)

Assessment: use both. The end state catches wrong answers; the path catches unsafe or
expensive right ones. None of it works unless you record what the agent did at each step,
its prompts, tool calls, time, and cost, so you can look back. Builders call that recording
"tracing," and it is the starting point for both debugging and the online testing above.

## Sources

- Anthropic, *How we built our multi-agent research system* — https://www.anthropic.com/engineering/multi-agent-research-system
- OpenAI, *Evaluation best practices* — https://developers.openai.com/api/docs/guides/evaluation-best-practices
- Google Cloud, *Evaluate AI agents with Vertex Gen AI evaluation service* — https://cloud.google.com/vertex-ai/generative-ai/docs/models/evaluation-agents
- *Judging the Judges: A Systematic Study of Position Bias in LLM-as-a-Judge* (arXiv:2406.07791) — https://arxiv.org/abs/2406.07791
- *CalibraEval* (arXiv:2410.15393) — https://arxiv.org/pdf/2410.15393
- Confident AI, *LLM Agent Evaluation Metrics* — https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide
