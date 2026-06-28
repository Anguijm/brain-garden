---
type: lesson
series: ai-engineering
title: "Thinking" and reasoning models
status: curated
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly]
created: 2026-06-29
---

# "Thinking" and reasoning models

Some models can stop and work through a problem before they answer. People call these
"reasoning models," or say the model is "thinking." This chapter explains what that
means, when it helps, and what it costs, in plain terms.

## What "thinking" actually means

Remember the basic trick from [the foundation chapter](00-what-is-an-llm): a model
guesses the next word, over and over. A reasoning model does the same thing, but first it
writes out a private chain of steps for itself, like scratch work on paper, and only then
gives you the final answer.

FACT: this extra step is often called "extended thinking" or "chain of thought." The
model reasons through the problem step by step before responding, which tends to improve
answers on harder tasks. (Anthropic, *Extended thinking*.)

You can get a light version of this from any model just by asking, "think step by step
before you answer." Reasoning models build that habit in and do a lot more of it.

## When it helps

Thinking pays off when a problem has several steps or needs careful logic. Good examples
are math word problems, puzzles, planning, debugging code, or any question where one
wrong step early ruins the answer. The model catches more of its own mistakes when it
works them out instead of blurting the first guess.

## When it is overkill

For simple tasks, thinking is wasted effort. "What is the capital of France?" or "rewrite
this sentence" does not need a chain of reasoning. Using a reasoning model there just
makes you wait longer and pay more for the same answer.

## What it costs

The thinking is not free. FACT: the model writes out those reasoning steps as text, and
that text counts as tokens you pay for, even though you may not see all of it. (Anthropic,
*Extended thinking*.) So reasoning models are slower and cost more per answer than plain
models.

Assessment: the simple rule is to match the tool to the job. Reach for a reasoning model
when the problem is genuinely hard and a wrong answer is costly. Use a plain, faster model
for everyday questions. This is the same "start simple" habit that runs through the whole
section.

## Sources

- Anthropic, *Extended thinking* — https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking
