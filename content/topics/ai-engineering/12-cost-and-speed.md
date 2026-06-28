---
type: lesson
series: ai-engineering
title: Cost and speed
status: curated
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly, finite-resources]
created: 2026-06-29
---

# Cost and speed

If you build anything real on a model, two practical things start to matter: how much it
costs and how fast it answers. Both come down to the same thing, the amount of text the
model has to handle. This chapter explains how you pay and how to keep both the bill and
the wait time down.

## You pay by the token

Recall from [the foundation chapter](00-what-is-an-llm) that models read and write in
small chunks called tokens (about three-quarters of a word each).

FACT: you are charged per token, and both directions count, the tokens you send in (your
prompt and any background) and the tokens the model writes back. (Anthropic, *Pricing*.)
Input is usually cheaper per token than output, but both add up.

![Diagram: tokens going in plus tokens coming out equals what you pay; a long input means a bigger bill and a longer wait](img/cost-tokens.png)
*Both the text you send and the text you get back cost money. Diagram.*

## Longer means pricier and slower

This is the key habit to build. A long prompt costs more, takes longer to answer, and (as
[the context chapter](04-context-engineering) explains) can make the model lose track of
the middle. So dumping in everything you have is the worst of all worlds: more money,
more waiting, and often a worse answer.

The same goes for output. Asking for a 2,000-word essay when you need a short summary
costs more and takes longer for no gain.

## Speed: bigger and "thinking" models are slower

Two things mostly drive how long you wait. Bigger, smarter models are slower than small
ones. And reasoning models (see [the reasoning chapter](11-reasoning-models)) are slower
still, because they do extra thinking before they answer. For a quick, simple task, a
smaller model often answers in a fraction of the time and is plenty good enough.

## Simple ways to spend less

Assessment: a few habits cut cost and wait time at once.

- **Keep prompts tight.** Send only what the model needs, not your whole document.
- **Match the model to the job.** Use a small, fast model for easy work; save the big or
  reasoning models for hard problems.
- **Ask for shorter answers** when that is all you need.
- **Look things up instead of pasting everything.** Feeding in just the relevant facts
  (see [retrieval](05-retrieval-and-rag)) beats stuffing the whole window.
- **Reuse repeated text.** If you send the same big instructions every time, some
  providers let you "cache" them so you are not billed full price each call.

The theme here is the same one from the [finite-resources thread](../../connections/finite-resources):
the window is a budget, so spend it on what matters.

## Sources

- Anthropic, *Pricing* — https://www.anthropic.com/pricing
- Anthropic, *Prompt caching* — https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching
