---
type: lesson
series: ai-engineering
chapter: 5
title: Pictures and voice (multimodal)
status: curated
aliases: ["topics/ai-engineering/13-pictures-and-voice"]
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly]
created: 2026-06-29
---

# Pictures and voice (multimodal)

So far this section has treated models as text in and text out, but many newer models can
also take in pictures, and some can handle sound as well. The technical word for this is
"multimodal," which simply means "more than one kind of input." This chapter explains what
that adds and where it still falls short.

## What "multimodal" means

A "mode" is just a type of input or output, such as text, images, or audio, and a
multimodal model is one that can take in more than a single type. FACT: modern models such
as Claude can accept images alongside text in the very same prompt, so you can show one a
picture and then ask questions about it. (Anthropic, *Vision*.)

In everyday use, this means you can hand a model a photo, a screenshot, a scanned page, or a
chart, and ask it to read or explain whatever it sees there.

## What it is good for

- **Reading a screenshot or a photo of text.** Snap a picture of a form, a sign, or a
  receipt, and the model can tell you what it says.
- **Explaining a chart or a diagram.** Paste in a graph and ask what the overall trend is,
  or what one particular part of it means.
- **Describing a scene.** This is handy for quick captions, and genuinely useful as an aid
  for people with low vision.
- **Turning a sketch or a photo into a starting point.** You might show it a rough drawing
  and ask for the basic layout of a page based on it.

Some tools also add voice, where you talk, the tool listens, and it can talk back. Under the
hood, that is usually your speech turned into text, fed to the model as usual, and the
model's answer turned back into speech.

## Where it still falls short

Assessment: the same limits from the [foundation chapter](01-what-is-an-llm) still apply,
and pictures bring a few of their own.

- **It can still be wrong.** A model can misread a blurry photo or a cluttered chart with
  exactly the same calm confidence it brings to text, so check anything that actually
  matters.
- **It is not a measuring instrument.** Do not rely on it to count tiny items, read fine
  print perfectly, or pull exact measurements out of an image.
- **Privacy matters even more.** A single photo can contain faces, addresses, screens, or
  documents you never meant to share, so be careful about what you upload, and keep the
  [safety chapter](13-safety-and-best-practices) in mind.

Assessment: multimodal is genuinely useful for reading and explaining what sits inside an
image, as long as you treat its answers the way you treat every other answer it gives,
helpful and fast, but worth a second look whenever the stakes are real.

## Sources

- Anthropic, *Vision (using images with Claude)* — https://docs.anthropic.com/en/docs/build-with-claude/vision
