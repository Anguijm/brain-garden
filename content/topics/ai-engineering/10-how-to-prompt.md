---
type: lesson
series: ai-engineering
title: How to write a good prompt
status: curated
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly]
created: 2026-06-29
---

# How to write a good prompt

A prompt is just what you type to the model. It is your request, plus any background you
give it. Writing a clear prompt is the single easiest way to get better answers, and it
takes no technical skill. This chapter covers the habits that matter most.

The big idea is simple: the model is not a mind reader. It only knows what you tell it
and what it learned during training. The more clearly you say what you want, the better
the result.

## Be specific about what you want

Vague prompts get vague answers. The fix is to spell out the details.

A weak prompt is "write about dogs." A strong one is "Write three short tips for
house-training a puppy, aimed at a first-time owner." The second one tells the model the
topic, the length, the format, and who it is for. That is four useful hints instead of
none.

![Diagram: a vague prompt leads to a vague answer, while a specific prompt (topic, length, format, audience) leads to a useful answer](img/good-prompt.png)
*A little detail in the prompt goes a long way. Diagram.*

## Give it the facts it needs

The model cannot see your files, your screen, or your situation unless you include it. If
you want feedback on an email, paste the email. If the answer depends on your numbers,
give it your numbers. When you leave facts out, the model fills the gap with a guess, and
that is where wrong answers come from.

## Say the format you want

If you want a bullet list, ask for a bullet list. If you want a table, a short paragraph,
or "under 100 words," say so. You can also show a tiny example of the shape you want, and
the model will copy that shape. FACT: giving an example or two in the prompt is a
well-known way to steer the format and style of the answer. (Anthropic, *Prompt
engineering overview*.)

## Set the scene when it helps

Telling the model who to be can sharpen the answer. "Explain this like I am new to the
topic" gets a gentler answer than "explain this to an expert." This is not magic, but it
nudges the wording and the depth in a useful direction.

## Break big jobs into steps

If a request is large, split it. Ask for an outline first, then ask it to fill in each
part. Smaller steps are easier for the model to get right, and easier for you to check.
This is the same idea as the "chain" workflow in [chapter on workflows](01-workflows-vs-agents).

## Tell it what to do when unsure

The model would rather give a confident wrong answer than admit it does not know (see
[the foundation chapter](00-what-is-an-llm) on this). You can push back on that. Add a
line like "If you are not sure, say so instead of guessing." It will not catch every
mistake, but it helps.

## Just try again

Your first prompt rarely needs to be perfect. If the answer misses, tell the model what
to fix: "shorter," "more formal," "you got the date wrong, it was June." Treating it as a
back-and-forth almost always beats trying to write one perfect prompt up front.

## The short version

Assessment: a good prompt usually has four things, what you want, the facts it needs, the
format you want, and what to do if it is unsure. Say those plainly and you will get more
out of any model, without learning a single technical trick.

## Sources

- Anthropic, *Prompt engineering overview* — https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
