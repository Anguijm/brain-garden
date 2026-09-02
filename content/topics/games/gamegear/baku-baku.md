---
title: "Baku Baku — Game Gear Guide"
type: game-guide
category: games/gamegear
tags: [gamegear, sega, baku-baku, puzzle, walkthrough, guide]
platform: Game Gear
year: 1996
created: 2026-09-02
draft: false
---

# Baku Baku (Game Gear)

**Developer:** Sega | **Publisher:** Sega | **Year:** 1996

---

## Introduction

Baku Baku — full title *Baku Baku Animal* — is a falling-block puzzle game with a mechanic nobody else used: **animals eat food.**

Blocks fall in pairs. Some are animals, some are food. An animal placed next to its matching food eats it, and while eating, it eats *every connected piece of that food* in a chain. Clear enough at once and you dump garbage on your opponent.

It is a competitive puzzler in the Puyo Puyo mould, and it is genuinely one of the better ones. The eating mechanic makes chain-building feel different from anything in Tetris or Puyo: you are not matching colours, you are building a buffet and then releasing a predator into it.

---

## Controls

- **D-Pad Left/Right:** Move the falling pair.
- **D-Pad Down:** Drop faster.
- **Button 1 / 2:** Rotate the pair.
- **Start:** Pause.

---

## The four pairings

Each animal eats exactly one food. Get these wrong and nothing happens.

| Animal | Eats |
|---|---|
| **Monkey** | Bananas |
| **Rabbit** | Carrots |
| **Panda** | Bamboo |
| **Dog** | Bones |

---

## How eating works

Place an animal adjacent to its food and it begins eating. It consumes the whole **connected mass** of that food — so a single rabbit dropped onto a column of eight carrots clears all eight.

That is the core strategy. You do not want to eat immediately; you want to **stockpile food** into a large connected block, then drop the matching animal on it for one enormous clear.

**Chains** happen when the pieces above collapse after a clear and create a new animal-food adjacency, which triggers again. Chains are what send meaningful garbage to your opponent.

---

## Strategy

**Build food, hold animals.** The commonest beginner mistake is eating as soon as possible. Small clears do almost nothing. Let carrots pile into a tall connected column and the payoff multiplies.

**Keep food types separated by column.** Mixing bananas and bones in the same area means an animal eats a small pocket rather than a large mass. Dedicate regions of the board.

**Watch what is coming.** The next pair is previewed. Placement decisions should account for it, especially when an animal is about to arrive and you are not ready.

**Garbage blocks are cleared by adjacency**, so an incoming dump is answered by triggering a clear beneath it rather than by digging.

**Do not stack to the ceiling.** The board is small on Game Gear, and a tall stockpile that gets interrupted by an unwanted animal is how games are lost.

---

## Emulation Notes

Runs perfectly on Batocera's Game Gear core. Puzzle games are the best case for **integer scaling** at 4K on this box — the block grid stays sharp and unambiguous, which matters when you are reading the board quickly.

**Rewind is enabled**, though in a puzzle game it is close to cheating; using it to undo a misplaced piece removes the game.

Two-player was link-cable on real hardware. In emulation the practical equivalent is the versus mode against the CPU. Note the ROM is `Baku Baku (USA)` — the full Japanese title is *Baku Baku Animal: Sekai Shiiku-gakari Senshuken*, and the game also appeared on Saturn and in arcades.
