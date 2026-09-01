---
title: "Super Mario Bros. — NES Guide"
type: game-guide
category: games/nes
tags: [nes, nintendo, mario, super-mario-bros, platformer, walkthrough, guide]
platform: NES
year: 1985
created: 2026-09-01
draft: false
---

# Super Mario Bros. (NES)

**Developer:** Nintendo R&D4 | **Publisher:** Nintendo | **Year:** 1985

---

## Introduction

Super Mario Bros. is the game that rescued the console business after the 1983 crash, and it is still worth playing rather than merely respecting. Everything it does, it does in service of one idea: that moving should feel good. Mario has momentum, he skids when you reverse, his jump height depends on how long you hold the button, and his air control is deliberately imperfect. Learning the game is learning that physics.

Eight worlds of four levels each. No save, no password on the original cartridge, no checkpoints between lives beyond the mid-level flag. A full run takes about half an hour once you know it, and most of that knowledge is muscle memory rather than secrets.

---

## Controls

- **D-Pad:** Move. Holding a direction accelerates rather than setting speed; Mario has real inertia.
- **A:** Jump. **Hold it.** Jump height is proportional to how long the button is held, up to about half a second. Tapping gives a hop; holding gives the full arc.
- **B:** Run (hold). Also fires a fireball with the Fire Flower.
- **B + direction:** Run. Mario's top speed is roughly double his walk, and several gaps in later worlds cannot be cleared without a running start.
- **Start:** Pause.

### The one thing that separates players

**Hold B almost always.** New players walk, find the later jumps impossible, and conclude the game is unfair. Running is the default state; walking is for precision. The corollary is that you must learn to stop, because at running speed Mario skids for most of a tile before reversing.

---

## Power-ups

**Super Mushroom** — Mario grows. The real benefit is a free hit: taking damage as Super Mario reverts you to small rather than killing you. Being big also lets you break bricks from below.

**Fire Flower** — Fireballs on B. Two on screen at a time, they bounce along the ground, and they kill almost everything in one hit. The most useful power-up in the game.

**Starman** — Brief invincibility, and you kill enemies by touching them. Chaining eight enemy kills during one star gives an extra life.

**1-Up Mushroom** — Extra life. Several are hidden in blocks that give no visual clue.

---

## World-by-world notes

**World 1** teaches the vocabulary: Goombas, Koopas, pipes, the flagpole. Hit the flagpole high for more points, though points only matter for extra lives.

**World 2** introduces the underwater level (2-2) and the first warp zone (in 4-2 and via the pipe at the end of 1-2). Bloopers in water cannot be killed without fire.

**World 3** brings night levels and the first serious platforming over gaps. 3-1 has the famous two-stair 1-up trick.

**World 4** introduces Lakitu, who throws Spinies from above and cannot be outrun, only outlasted or killed during his brief low passes.

**World 5** repeats World 1's shapes at higher speed and with worse footing.

**World 6** has the long bridge sections and the first genuinely tight jump sequences.

**World 7** is a difficulty step: Hammer Bros. appear in pairs and their arcs are hard to read. The reliable answer is to stand still under them and jump the moment they jump, or to run past low while they are airborne.

**World 8** is the endurance test. 8-1 is a long run with pit jumps at speed, 8-2 adds Hammer Bros. on narrow platforms, 8-3 is a corridor of them, and 8-4 is a maze castle where taking the wrong pipe loops you back. The correct 8-4 route: in the first water room take the **fourth** pipe, then in the next area take the pipe at the end, then the second-to-last.

---

## Bowser and the fake Bowsers

Every castle ends with a Bowser sprite. In Worlds 1 through 7 it is a disguised ordinary enemy, revealed when you cross the axe. You have two options in every castle:

1. **Fireballs**, if you have them: several hits and he drops.
2. **Run underneath.** Bowser jumps on a rhythm. Wait for him to leap, run under, and touch the axe. This works in every castle including 8-4 and costs nothing.

The axe drops the bridge and kills him regardless of health, so the run-under is always available and always faster.

---

## Warp zones

- **World 1-2:** run along the ceiling at the end (climb the pipe area, run right across the top) to reach warps for Worlds 2, 3 and 4.
- **World 4-2:** a beanstalk in a hidden block leads to a platform and the Warp Zone for World 5; there is also a ceiling route to World 8.

Warping to World 8 from 4-2 turns a half-hour game into about ten minutes, at the cost of skipping the middle of the game.

---

## The minus world

The best-known bug in console history. At the end of 1-2, stand on the pipe, crouch into the wall at the right angle and Mario passes through the barrier into the Warp Zone from the wrong side, sending you to World "-1", an endlessly looping water level. It is a bug rather than a secret, it exists only on the original Japanese and North American cartridges, and it cannot be escaped except by dying.

---

## Quick Reference

| Situation | Answer |
|---|---|
| Later jumps feel impossible | You are walking. Hold B. |
| Bowser, any castle | Wait for his jump, run under, touch the axe |
| Lakitu will not leave | Kill him when he dips low, or outrun the level |
| Hammer Bros. | Jump when they jump, or pass low while airborne |
| 8-4 pipe maze | Fourth pipe, then end pipe, then second-to-last |

---

## Emulation Notes

Super Mario Bros. is trivial to emulate and runs perfectly on Batocera's FCEUmm core. Two settings are worth having: **rewind** (enabled on this box), which turns the no-continues structure into something you can actually learn from, and **integer scaling** so the 256x240 image lands on the 4K panel as uniform pixel blocks.

Note the ROM is `(World)` rather than `(USA)`: Nintendo shipped one cartridge image worldwide for this title, so that is the correct North American file.
