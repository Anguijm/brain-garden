---
type: lesson
series: ai-engineering
chapter: 3
title: '"Thinking" and reasoning models'
status: curated
aliases: ["topics/ai-engineering/11-reasoning-models"]
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly]
created: 2026-06-29
---

# "Thinking" and reasoning models

Some models can pause and work through a problem before they answer, and people call these
"reasoning models," or describe the model as "thinking." This chapter explains what that
actually means, when it earns its keep, and what it costs, all in plain terms.

## What "thinking" actually means

Recall the basic trick from the [foundation chapter](01-what-is-an-llm): a model guesses
the next word, over and over. A reasoning model does exactly the same thing, except that it
first writes out a private chain of steps for itself, much like scratch work on paper, and
only then hands you the final answer.

FACT: this extra step is usually called "extended thinking," or "chain of thought." Because
the model reasons through the problem one step at a time before responding, its answers tend
to improve on the harder tasks. (Anthropic, *Extended thinking*.)

You can get a lighter version of the same effect from almost any model simply by asking it
to "think step by step before you answer." Reasoning models just build that habit in and do
a great deal more of it.

## When it helps

Thinking pays off when a problem has several moving parts or demands careful logic. Good
examples include math word problems, puzzles, planning a sequence of actions, and debugging
code, essentially any question where one wrong step early on ruins everything that follows.
The model catches far more of its own mistakes when it works them out deliberately than when
it blurts out the first guess.

## When it is overkill

For simple tasks, all that deliberation is wasted effort. A question like "what is the
capital of France?" or a request to "rewrite this sentence" needs no chain of reasoning, and
using a reasoning model there only makes you wait longer and pay more for the very same
answer.

## What it costs

The thinking is not free. FACT: the model writes those reasoning steps out as text, and that
text counts as tokens you pay for, even though you may never see all of it. (Anthropic,
*Extended thinking*.) As a result, reasoning models run slower and cost more per answer than
plain ones.

Assessment: the simple rule is to match the tool to the job. Reach for a reasoning model
when the problem is genuinely hard and a wrong answer would be costly, and fall back to a
plain, faster model for everyday questions. This is the same "start simple" habit that runs
through the whole section.

## Sources

- Anthropic, *Extended thinking* — https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking
