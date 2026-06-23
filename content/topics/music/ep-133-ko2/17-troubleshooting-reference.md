---
type: lesson
series: ep-133-ko2
chapter: 17
title: Troubleshooting, reset, and reference
status: curated
tags: [music, sampler, groovebox, teenage-engineering, ep-133, ko-ii, tutorial, reference]
created: 2026-06-23
---

# Chapter 17 — Troubleshooting and reference

The closing chapter: how to fix common problems, the reset procedure, a combo cheat
sheet, a glossary, and the sources behind this course.

## Common problems

- **Memory full (`FUL`) or low-disk warning.** Delete unused samples (hold `ERASE` +
  `SOUND` until `SND` blinks) or bounce parts together by
  [resampling](16-advanced-techniques.md). Sample short, design around one-shots.
- **The EP Sample Tool / updater won't connect.** Almost always a charge-only cable
  or a USB hub. Use a data-capable USB-C cable, plug straight into the computer, and
  use a WebUSB browser (Chrome). ([Chapter 15](15-samples-and-computer.md).)
- **No sync with other gear.** Pick one device as the clock master and set the rest
  to follow (MIDI clock codes 100/101/102 in the system menu). Don't have two
  masters. ([Chapter 14](14-midi-and-sync.md).)
- **A punch-in effect didn't get recorded.** It can't be; punch-in FX are
  performance-only. Resample to capture them. ([Chapter 10](10-effects.md).)
- **A pad won't stop choking another.** They're in the same mute group; change the
  mute-group assignment in sound edit. ([Chapter 7](07-shaping-sounds.md).)

## Reset and recovery

FACT: The official format/reset (per TE's erase-drive guide) is: power **off**, hold
`SHIFT` + `ERASE`, then power **on** while holding. The screen shows `FMT` for about
ten seconds and the device reboots empty. This erases **everything, including the
factory sounds** (which cannot be recovered), so back up with the EP Sample Tool
first. It is the documented fix for file-system error codes (E.05, E.10, E.11,
E.12). Hardware error codes (E.01-E.03) need TE service. (Some third-party guides
list a different combo for formatting after a filesystem error; trust the official
erase-drive page and **verify on device** before using any other combo.)

FACT: Lock mode (hold `MAIN` during startup) locks projects, patterns, pads, and
settings, and shows `lok` briefly. Useful before handing the unit to someone or
playing live; do the same combo to consider it engaged/disengaged per the system
menu.

## Combo cheat sheet

FACT unless noted. The general grammar: `SHIFT` reaches second functions, `ERASE`
removes, hold a mode button then use `-`/`+` and the knobs.

- `SOUND` — sound mode; `SHIFT` + `SOUND` — sound edit (hold ~2s to save)
- `SAMPLE` — sampling; `SHIFT` + `SAMPLE` — chop
- `RECORD` then `PLAY` — record with four-beat count-in; `RECORD` + `PLAY` — no
  count-in
- `RECORD` + pad — add a hit on the step; `ERASE` + pad — remove that pad's hits
- `TIMING` + `knob X` — note interval; `knob Y` — swing; hold `TIMING` + pad — note
  repeat; `SHIFT` + `TIMING` — timing correct
- hold `FADER` — see/assign the fader; `RECORD` + move `FADER` — record automation;
  `ERASE` + `FADER` (~2s) — clear fader data
- hold `FX` + pads — punch-in effects; `SHIFT` + `FX` — output (master compressor /
  sidechain)
- `SHIFT` + `MAIN` — commit (snapshot a scene); hold `MAIN` + `-`/`+` or number —
  switch scene; hold `MAIN` + pad — switch project
- hold a group + `-`/`+` or number — switch that group's pattern
- `RECORD` + `-`/`+` — pattern length; `SHIFT` + `-`/`+` — move between bars
- `SHIFT` + `ERASE` — system menu; hold `MAIN` + `ENTER` — song-list editor *(verify
  on device)*; `MAIN` + `TEMPO` — time signature *(verify on device)*
- power on holding `SHIFT` + `ERASE` — format/reset; power on holding `MAIN` — lock
  mode

## Glossary

- **Project** — one whole song's material; 9 on the device.
- **Group (A-D)** — a set of 12 sounds with its own patterns; 4 per project.
- **Pattern** — a sequence for one group, 1-99 bars, 99 per group.
- **Track** — one of the 12 lanes (one per pad) in a pattern.
- **Scene** — a snapshot of which pattern each group is playing; 99 per project.
- **Song** — a chain of up to 99 scenes.
- **Commit** — `SHIFT` + `MAIN`; snapshot the current state into a scene and keep
  building.
- **Punch-in FX** — momentary, pressure-sensitive performance effects (not
  recordable).
- **Group send FX** — the always-on effect for a group (delay, reverb, distortion,
  chorus, filter, compressor).
- **Resample** — record the device's own output (effects baked in) to a new sample.
- **Note repeat** — hold `TIMING` + pad for rolls/fills.
- **Mute group** — pads set to choke each other (e.g. hats).

## Sources

This course was synthesized chiefly from Teenage Engineering's official guide and
corroborated with published reviews and a third-party manual. Device behavior was
treated as FACT when it appears in the official guide; a few combos and figures that
only appear in secondary sources are flagged "(verify on device)" in the text.

- Teenage Engineering official guide: https://teenage.engineering/guides/ep-133
  (and its subpages: get-started, modes, workflow, play-and-record, functions,
  effects, system, screen, buttons-and-combos, whats-new, erase-drive)
- Teenage Engineering product/specs: https://teenage.engineering/products/ep-133
- EP Sample Tool: https://teenage.engineering/apps/ep-sample-tool ; updater:
  https://teenage.engineering/apps/update
- Sound on Sound review: https://www.soundonsound.com/reviews/teenage-engineering-ep-133-ko-ii
- MusicRadar review and OS 2.0 coverage: https://www.musicradar.com
- SineSquares hands-on review: https://www.sinesquares.net/musicgear/teenage-engineering-ep-133-ko-ii-review
- Teenage Engineering official YouTube tutorial series ("first beat", "scenes &
  patterns", "sampling & chopping", "sequencer") and the sampling deep-dives
- teenagemanual.com/ep-133 (third-party manual; used for some combos, flagged where
  unverified)

Next: [Recipes](18-recipes.md).
