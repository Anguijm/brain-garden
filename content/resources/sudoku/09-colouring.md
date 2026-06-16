---
type: lesson
series: sudoku
lesson: 9
title: Colouring — single-digit chains
status: curated
created: 2026-06-16
---

# Lesson 9 — Colouring (single-digit chains)

Colouring is the gateway to chain logic, and it stays on a single digit so it's the
easy way in. It rests on the **conjugate pair**: a unit where a digit has exactly two
possible cells. In such a pair, one is the digit and the other isn't, a clean
on/off switch.

## The method

Pick a digit. Find every conjugate pair for it (units where it has exactly two
spots). Treat each pair as a link, and colour the two ends opposite colours, say
blue and green. Because they're a true on/off switch, every blue cell is the same
truth value, and every green cell is the opposite. Chain the colours across the grid:
whenever a coloured cell forms a conjugate pair with a new cell, that new cell takes
the opposite colour. You end up with two colour groups for the digit.

## The two eliminations

1. **Colour clashes with itself.** If two cells of the *same colour* see each other,
   that colour can't be the true one (it would put the digit twice in one unit). So
   the whole colour is false, and **every cell of that colour loses the digit**; the
   other colour is the answer everywhere it appears.

2. **A cell sees both colours.** If an uncoloured candidate cell sees a blue cell and
   a green cell, then whichever colour is true, this cell is in view of the digit. So
   **erase the digit from that cell.**

## How to spot it

Use this when a digit is stubborn and you can find a network of conjugate pairs for
it. It's a single-digit technique, which keeps it manageable: you're only ever
tracking one number. Look first for a same-colour clash (it solves the digit
outright), then for cells caught between the two colours.
