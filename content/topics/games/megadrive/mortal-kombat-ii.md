---
title: "Mortal Kombat II — Mega Drive Guide"
type: game-guide
category: games/megadrive
tags: [megadrive, genesis, mortal-kombat, midway, acclaim, fighting, walkthrough, guide]
platform: Sega Mega Drive / Genesis
year: 1994
created: 2026-09-02
draft: false
---

# Mortal Kombat II (Mega Drive / Genesis)

**Developer:** Probe Software / Midway | **Publisher:** Acclaim | **Year:** 1994

---

## Introduction

The Mega Drive version of *Mortal Kombat II*, and the version that mattered in 1994 — because unlike the first game, where Nintendo censored the blood out and Sega hid it behind a code, **both console versions of MKII shipped uncensored**. The ratings board that Mortal Kombat itself had caused into existence had done its work.

Twelve fighters, a tournament in Outworld, and the fatalities that made it notorious.

---

## Controls, and the button problem

Mortal Kombat II uses **five attack buttons**: high punch, low punch, high kick, low kick, and block.

**The standard Mega Drive pad has three.** That is a genuine hardware shortfall, not a configuration mistake, and it is why this game feels wrong on a default setup — the three-button pad forces the game into a mode where punches and kicks share buttons and the ones you want are missing.

**This box is configured for the six-button Mega Drive pad**, which Sega released in 1993 specifically because of games like this one. On your controller that gives you all five attacks plus block mapped separately, the way the arcade had it:

| Function | Six-button pad |
|---|---|
| **High punch** | X |
| **Low punch** | A |
| **High kick** | Y |
| **Low kick** | B |
| **Block** | C or Z |

If the punches ever collapse back into one button, the emulator has fallen back to the three-button pad and the setting to check is the Mega Drive controller device type.

---

## Fundamentals

**Block is a button, not a direction.** Holding back does not block in Mortal Kombat. This is the single biggest adjustment for anyone coming from [Street Fighter](topics/games/snes/street-fighter-ii-turbo), and it changes everything: you can block while walking forward, and you cannot block by retreating.

**Sweeps and uppercuts** are the bread and butter. Down + low kick sweeps; down + high punch uppercuts and does heavy damage.

**Jump kicks control space.** Most rounds at a casual level are decided by jump kicks and uppercuts, not specials.

---

## Special moves

Each fighter has three or four, mostly quarter-circle and back-forward motions plus a button. A few examples:

- **Scorpion:** back, back + low punch — the spear ("get over here")
- **Sub-Zero:** down, forward + low punch — freeze
- **Liu Kang:** forward, forward + high punch — flying kick; forward, forward + low punch — fireball
- **Raiden:** back, back, forward + low punch — torpedo

---

## Fatalities, and the rest

Finishing moves are entered on the "Finish Him" prompt at a specific distance, with a specific input, within a few seconds. There are also **Babalities**, **Friendships** and **stage fatalities** — the pit and the spikes.

They are all input-code driven and effectively unguessable. Looking them up is the normal way anyone has ever learned them.

---

## Strategy

**Learn to use the block button early.** Everything else follows from it.

**Uppercut anything that jumps at you.**

**Practise one character's specials rather than sampling everyone.**

**Sweep after a blocked attack.** The recovery windows here are generous.

---

## Emulation Notes

Runs perfectly on Batocera's Mega Drive core.

**The six-button pad setting is the whole game.** This box sets `input_libretro_device_p1` and `p2` to the six-button pad for the Mega Drive system, which is what makes low punch and high punch separate buttons. Without it the game is playable but not the game it was designed as.

Rewind is enabled for the system, but is of little use in a fighting game.
