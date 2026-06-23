---
type: lesson
series: ep-133-ko2
chapter: 16
title: Advanced techniques and recipes
status: curated
tags: [music, sampler, groovebox, teenage-engineering, ep-133, ko-ii, tutorial]
created: 2026-06-23
---

# Chapter 16 — Advanced techniques and recipes

![The SAMPLE button, used for resampling](img/sample.jpg)
*Resampling uses SAMPLE with RSP as the source. Photo: Teenage Engineering.*

This chapter puts the pieces together into workflows that get more out of the device,
especially resampling, which is the single most powerful "advanced" feature.

## Resampling: the master technique

FACT: Resampling records the K.O. II's own output, with effects baked in, back onto
a pad. Set the input source to `RSP` (mono or stereo) in sample mode, then capture
(hands-free is easiest: `SHIFT` + an empty pad, then `PLAY` to record the current
pattern's length). What you get is a new sample that is the bounced sum of whatever
was playing plus its effects.

Why it matters (FACT): it lets you

- **bounce a stack into one sample** to free up voices and memory (you only have 16
  mono / 12 stereo voices),
- **commit effects permanently** into audio, including the punch-in FX that can't be
  recorded any other way ([chapter 10](10-effects.md)),
- **capture a performance** (a filter sweep, a stutter, a chopped phrase) as a single
  reusable sound.

Assessment: a strong production loop is build → resample → simplify → build again.
Make a busy four-bar idea, resample it to one pad, and now you have a whole group and
most of your voices back to add the next layer. Many finished K.O. II tracks are
several resampling passes deep.

## Recipe: bake in a reverse or a filter envelope

Assessment: the device has no sample reverse and no filter envelope
([chapter 7](07-shaping-sounds.md)). Get them anyway by performing the move and
resampling it: play a sound through the group filter while sweeping the fader, or
through a punch-in effect, and resample the result. The new sample now "has" the
sweep or the reverse-like swell baked in, and you can sequence it normally.

## Recipe: lo-fi via sample rate and speed

FACT/Community: OS 2.0 can store samples at lower rates (down to 11.025 kHz) to save
space and add grit. A community trick is to play a source at 2x or 4x speed into the
K.O. II, sample it, then it plays back at normal speed using a quarter of the memory
with a characterful lo-fi texture (flagged as a community technique, not official).

## Recipe: chop, re-sequence, resample

Assessment: take a melodic loop, auto-chop it across a group
([chapter 6](06-chopping.md)), re-sequence the slices into a new melody rather than
the original order, add a group effect, then resample the result to one pad. You have
turned someone else's phrase into your own instrument and freed the group to do it
again.

## Recipe: sidechain pump without a kick in the way

FACT: The sidechain can be triggered by note events even when the source isn't
audible ([chapter 11](11-mixing-and-master.md)). Put a silent or very quiet trigger
pattern on a pad and use it to pump the bass or pads rhythmically, giving you the
classic ducking groove with full control over the rhythm of the pump.

## Performance-to-production bridge

Assessment: the through-line of all of this is that the K.O. II rewards a
performance-then-capture mindset. The live-only features (punch-in FX, pressure,
fader moves) become permanent when you resample them. Treat resampling as the
"render" step and you can build arrangements far more complex than the voice count
and the per-pad limits would suggest.

Next: [Troubleshooting and reference](17-troubleshooting-reference.md).
