---
type: lesson
series: ep-133-ko2
chapter: 5
title: Sampling — mic, line, and resample
status: curated
tags: [music, sampler, groovebox, teenage-engineering, ep-133, ko-ii, tutorial]
created: 2026-06-23
---

# Chapter 5 — Sampling

Recording your own audio is the whole point of a sampler. The K.O. II makes it fast.

## The format, so you know your limits

FACT: Sampling is **16-bit at 46.875 kHz**, in **mono or stereo**, into **999
slots** backed by **128 MB** of memory. The internal signal path is 32-bit float
with 24-bit converters, so it sounds clean. A single sample can run up to roughly
**40 seconds** on the 128 MB model (widely reported; the exact ceiling is not stated
in the official spec, so treat 40s as a guideline, **verify on device**). OS 2.0 can
also store samples at lower rates (down to 11.025 kHz) to fit far more lo-fi audio
in memory.

## The input sources

FACT: Press `SAMPLE` to enter sampling mode (the pads light up to show they are
ready). Use `-` / `+` to choose the input source. The options are:

- `MIC` — the built-in microphone,
- `IN` — line input, mono or stereo (3.5 mm jack),
- `L.IN` / `R.IN` — just the left or right channel of the input as mono,
- `RSP` — resample, mono or stereo (records the device's own output).

`knob X` sets the input level and `knob Y` sets a threshold (recording can start
when the input crosses that level).

## Recording a sample, step by step

FACT:

1. Choose where it lands (optional but tidy): hold `SOUND`, type a slot number,
   press `ENTER`. Otherwise the new sample drops into the next free slot.
2. Press `SAMPLE` and pick the input with `-`/`+`.
3. Set level with `knob X` while watching/listening; set threshold with `knob Y` if
   you want it to wait for sound.
4. **Manual capture:** press and hold a pad to record, release to stop. Press the
   pad again to audition.
5. Not happy? Press `SAMPLE` again and redo it.
6. Press `MAIN` to leave sampling mode.

## Hands-free sampling (OS 2.0)

FACT: To sample with both hands free (handy for playing an instrument while you
record), in sample mode hold `SHIFT` and press a pad to "latch" it, then press
`PLAY`. The device records for the length of the current pattern; set how many bars
with `-`/`+`. Press `SAMPLE` to stop. This is also the clean way to capture a whole
loop, and it is the basis of resampling below.

## Resampling in one line (more in chapter 16)

FACT: Set the source to `RSP` and the K.O. II samples its own output, **with effects
baked in**. That lets you bounce a stack of sounds and effects down into a single
new sample, freeing up voices and memory. The full workflow is in
[chapter 16](16-advanced-techniques.md).

## Managing memory

FACT: Every sample you take is stored in the shared library and auto-numbered. When
`FUL` shows or the low-disk warning appears (under ~20 seconds left), delete unused
samples (hold `ERASE` + `SOUND` until `SND` blinks) or bounce parts together by
resampling. Back up anything precious first with the EP Sample Tool
([chapter 15](15-samples-and-computer.md)).

Assessment: sample short and deliberately. This is a groovebox, not a field
recorder; tight one-shots and short loops are where it shines, and they keep you
from filling memory mid-session.

Next: [Chopping](06-chopping.md).
