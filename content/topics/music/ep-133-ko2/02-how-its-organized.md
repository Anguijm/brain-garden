---
type: lesson
series: ep-133-ko2
chapter: 2
title: How it's organized — projects, groups, patterns, scenes
status: curated
tags: [music, sampler, groovebox, teenage-engineering, ep-133, ko-ii, tutorial]
created: 2026-06-23
---

# Chapter 2 — How it's organized

Before you make a beat, it helps to hold the device's mental model in your head.
Everything nests in a clear hierarchy.

## The hierarchy (top to bottom)

FACT, from the official guide:

- **Project** = one whole song's worth of material. The device holds **9
  projects** (projects 1-5 ship with demo content, 6-9 are empty). Each project can
  hold up to 80,000 notes.
- **Group** = a collection of 12 sounds with its own sequences. Every project has
  **4 groups**, labelled `GROUP A` to `GROUP D`. A common convention (not enforced)
  is A = drums, B = bass, C = melodies, D = loops.
- **Pattern** = a sequence for one group. Each group can hold up to **99 patterns**,
  and a pattern can be **1 to 99 bars** long. Each group runs its own pattern, so
  the four groups can be on different patterns of different lengths at the same
  time.
- **Track** = one lane within a pattern. A pattern has **12 tracks**, one per pad.
  So in any group, pad 1 is track 1, pad 2 is track 2, and so on. (The official
  prose calls these "12 samples sequenced in a pattern"; the spec sheet calls them
  "12 tracks." Same thing.)
- **Scene** = a snapshot of which pattern is active in each of the four groups. A
  project holds up to **99 scenes**. Scenes are how you build song sections (intro,
  verse, chorus) and switch between them live.
- **Song** = a chain of scenes. Song mode plays through up to **99 positions**,
  each holding a scene, so a song can run up to 99 x 99 = 9,801 bars.

Assessment: the cleanest way to picture it is a grid. Down the side are the four
groups; each group has a stack of patterns; a scene is one "column" that picks one
pattern from each group; a song is a left-to-right playlist of those columns.

## Where samples actually live

FACT: Samples live in a single **sound library** shared across the whole project,
with **999 numbered slots**. The factory library is organized by hundreds: kicks
from 1, snares from 100, hi-hats from 200, percussion from 300, bass from 400,
melodic from 500. When you put a sound on a pad, you are **pointing that pad at a
slot**, not copying the audio. That means:

- the same sample can sit on many pads or in many groups without using extra
  memory,
- editing a pad's parameters (pitch, trim, envelope) does **not** change the stored
  audio, only how that pad plays it,
- you free memory by deleting the underlying sample from the library, not by
  clearing a pad.

A project can therefore reference up to 48 sounds on pads (4 groups x 12 pads) while
the library behind them holds far more.

## Polyphony (how many notes at once)

FACT: The K.O. II plays up to **16 mono or 12 stereo voices** at once (raised in OS
2.0). Past that it "steals" the oldest voice. This matters when you stack long
samples or dense patterns; one fix is to bounce parts down by resampling (see
[chapter 16](16-advanced-techniques.md)).

## Saving and switching

FACT: Work persists per project on the device; you switch projects by holding
`MAIN` and pressing a pad (1-9). The main way you "save as you go" is **commit**
(`SHIFT` + `MAIN`), which snapshots the current arrangement into a scene so you can
keep building without overwriting it (covered in
[chapter 12](12-arranging.md)). To back work up off the device, use the EP Sample
Tool over USB (see [chapter 15](15-samples-and-computer.md)).

Next: [Your first beat](03-your-first-beat.md).
