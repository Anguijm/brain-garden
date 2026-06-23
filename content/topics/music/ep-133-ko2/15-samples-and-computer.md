---
type: lesson
series: ep-133-ko2
chapter: 15
title: Samples and your computer — transfer, backup, firmware
status: curated
tags: [music, sampler, groovebox, teenage-engineering, ep-133, ko-ii, tutorial]
created: 2026-06-23
---

# Chapter 15 — Samples and your computer

Getting audio on and off the device, backing up your work, and keeping firmware
current all happen over USB-C through Teenage Engineering's browser tools.

## The EP Sample Tool

FACT: The K.O. II does **not** mount as a USB drive and has no SD card. Instead you
use the **EP Sample Tool**, a browser app (teenage.engineering/apps/ep-sample-tool)
that connects over USB-C using WebUSB. With it you:

- drag audio files from your computer into sample slots,
- assign samples to pads by dragging onto an on-screen K.O. II,
- pull samples off the device, and
- let it auto-convert formats, so you don't have to prepare files first.

What you need: a WebUSB-capable browser (Chrome works), a **data-capable** USB-C
cable (not a charge-only one), and ideally a direct port rather than a hub. Most
"it won't connect" problems are a charge-only cable or a hub.

FACT: It is **not** a USB audio interface; USB carries MIDI and sample-tool data,
not streamed audio. To get a finished track into your computer as audio, record the
K.O. II's output jack into an interface, or resample and transfer the file.

## Backing up

FACT: Because a factory reset erases everything including the factory sounds, back
up before you reset and back up work you care about. The EP Sample Tool is the way to
pull your samples to the computer. Assessment: get in the habit of exporting your
sample library after a productive session; the device is robust, but memory is finite
and resets are sometimes the fix for a glitch.

## Firmware updates

FACT: Update over USB-C with Teenage Engineering's browser updater
(teenage.engineering/apps/update); no drivers needed. The current line is OS 2.0.x
(2.0.5 at the time of writing), and a normal firmware update does **not** wipe your
content. The version shows on the startup screen and in the system menu.

Assessment: update before you start learning in earnest, both so this course matches
your unit and because OS 2.0 added the big features (resampling, song mode,
sidechain, hands-free sampling, MIDI thru). Use a good cable and a powered port, and
don't interrupt an update mid-flash.

Next: [Advanced techniques](16-advanced-techniques.md).
