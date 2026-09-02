---
title: "Tetris — Game Boy Guide"
type: game-guide
category: games/gb
tags: [gb, gameboy, nintendo, tetris, puzzle, walkthrough, guide]
platform: Game Boy
year: 1989
created: 2026-09-02
draft: false
---

# Tetris (Game Boy)

**Developer:** Nintendo R&D1 | **Publisher:** Nintendo | **Year:** 1989

---

## Introduction

This is the version that sold the Game Boy. Bundling it was the decision that took a monochrome handheld into offices, aeroplanes and every household that had no interest in Mario — and it remains one of the purest games ever made.

It is worth being clear about which Tetris this is, because the rules differ meaningfully from modern versions. **There is no hold, no hard drop, no ghost piece, and no wall kicks.** Rotation is simple and unforgiving. If you have learned Tetris on a modern client, several habits will not work here.

Two modes: A-Type, endless, and B-Type, clear 25 lines at a set starting height.

---

## Controls

- **D-Pad Left/Right:** Move.
- **D-Pad Down:** Soft drop, one row at a time. **There is no hard drop.**
- **A / B:** Rotate clockwise / anticlockwise.
- **Start:** Pause.

---

## What is different from modern Tetris

**No hold queue.** You cannot bank an I-piece for later. What arrives must be placed.

**One-piece preview.** You see the next piece only, not five.

**Random is genuinely random.** Modern Tetris uses a "bag" system guaranteeing all seven pieces appear before any repeats. This one does not — you can and will get long droughts of the piece you need. Building a well four rows deep and waiting for an I-piece is a gamble here in a way it is not in a modern game.

**No wall kicks.** A rotation that would clip a wall or a stack simply fails. Rotating flush against the right wall often does nothing.

**Gravity does not lock instantly.** You get a brief moment after landing, but far less generosity than modern lock delay.

---

## Scoring, and why line-clear size matters

| Lines cleared at once | Relative value |
|---|---|
| Single | 40 x level |
| Double | 100 x level |
| Triple | 300 x level |
| **Tetris (4)** | **1200 x level** |

A Tetris is worth thirty times a single at the same level. That is the whole scoring strategy: build flat, keep one column open, drop the I-piece.

---

## Strategy

**Keep the stack flat.** Every hole you bury costs you lines later. Placing a piece badly to save two seconds is nearly always wrong.

**Leave the well on the right.** By convention, keep column 10 open for I-pieces. It matters less which side than that you are consistent.

**Do not chase Tetrises at high level.** Because piece distribution is truly random, waiting for an I-piece at speed is how you top out. Above roughly level 12, take doubles and triples and stay alive.

**Soft-drop early, not late.** With no hard drop, holding Down is how you gain time. Decide the column while the piece is high.

**Rotate before you move.** With no wall kicks, rotating against a wall fails silently. Set the orientation in open space, then slide it over.

---

## B-Type and the rocket

B-Type asks for 25 lines at a chosen starting garbage height. Clearing it shows a rocket launch, and the rocket gets bigger with higher difficulty and height settings — the closest thing the game has to an ending.

---

## Emulation Notes

Trivial to emulate; runs perfectly on Batocera's Game Boy core. **Integer scaling** matters here more than the art might suggest: 160x144 divides cleanly into 4K, so the blocks stay square and unambiguous.

**Rewind is enabled** on this system, and in Tetris it is close to meaningless — undoing a misplacement removes the game entirely. Worth leaving alone.

The ROM is `Tetris (World) (Rev 1)`. Rev 1 fixes a bug in the original printing; it is the version to have. Note this is the Nintendo Game Boy release, not the later *Tetris DX* on Game Boy Color.
