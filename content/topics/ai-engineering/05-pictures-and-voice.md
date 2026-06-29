---
type: lesson
series: ai-engineering
title: Pictures and voice (multimodal)
status: curated
aliases: ["topics/ai-engineering/13-pictures-and-voice"]
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly]
created: 2026-06-29
---

# Pictures and voice (multimodal)

So far this section has treated models as text in, text out. But many newer models can
also handle pictures, and some can handle sound. The fancy word for this is
"multimodal," which just means "more than one kind of input." This chapter explains what
that adds and where it still falls short.

## What "multimodal" means

A "mode" is a type of input or output: text, images, audio. A multimodal model can take
in more than one type. FACT: modern models such as Claude can accept images alongside
text in the same prompt, so you can show one a picture and ask about it. (Anthropic,
*Vision*.)

In everyday use this means you can hand a model a photo, a screenshot, a scanned page, or
a chart, and ask it to read or explain what it sees.

## What it is good for

- **Reading a screenshot or photo of text.** Snap a picture of a form, a sign, or a
  receipt and ask what it says.
- **Explaining a chart or diagram.** Paste a graph and ask what the trend is.
- **Describing a scene.** Useful for quick captions, or as a help for people with low
  vision.
- **Turning a sketch or photo into a starting point**, like asking for the rough layout
  of a page from a drawing.

Some tools also add voice: you talk, it listens, and it can talk back. Under the hood that
is usually speech turned into text, fed to the model, and the answer turned back into
speech.

## Where it still falls short

Assessment: the same limits from [the foundation chapter](01-what-is-an-llm) still apply,
and pictures add a few of their own.

- **It can still be wrong.** A model can misread a blurry photo or a messy chart with the
  same calm confidence it has about text. Check anything that matters.
- **It is not a measuring tool.** Do not trust it to count tiny items, read fine print
  perfectly, or give exact measurements from an image.
- **Privacy matters more.** A photo can hold faces, addresses, screens, or documents you
  did not mean to share. Be careful what you upload, and keep
  [the safety chapter](13-safety-and-best-practices) in mind.

Assessment: multimodal is genuinely useful for reading and explaining what is in an
image, as long as you treat its answers the way you treat any other answer, helpful, fast,
and worth a second look when it counts.

## Sources

- Anthropic, *Vision (using images with Claude)* — https://docs.anthropic.com/en/docs/build-with-claude/vision
