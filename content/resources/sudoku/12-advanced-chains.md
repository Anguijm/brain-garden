---
type: lesson
series: sudoku
lesson: 12
title: Advanced chains and last resort
status: curated
created: 2026-06-16
---

# Lesson 12 — Forcing chains, AICs, and the last resort

This is the deep end. You rarely need it, and most "expert" puzzles fall to
everything in Lessons 1 to 11. Here's the map so the words aren't mysterious.

## Forcing chains

Pick a cell with two candidates and test a consequence. "If this cell is 3, then by a
trail of forced moves, cell Z becomes 5." Then test the other option: "If this cell
is 8, then by a different trail, cell Z *also* becomes 5." If **both** of a cell's
options force the same result somewhere, that result is true regardless, so you can
place it. It's case-analysis done cleanly, following only forced steps, not guesses.

## AIC (Alternating Inference Chains) and Nice Loops

The formal grown-up version of every chain you've met. It alternates two link types:

- **Strong link:** "if not A, then B" (at least one is true), like a conjugate pair.
- **Weak link:** "if A, then not B" (at most one is true), like two candidates in the
  same unit.

Alternate strong, weak, strong, weak along a chain and you can prove eliminations or
placements. Colouring, X-chains, and XY-chains are all special cases of AICs. This is
the ceiling of pure logical solving; learn it last, and only if you enjoy it.

## Last resort: bifurcation (guess and check)

When even chains fail or you don't want to hunt them, pick a bivalue cell, copy the
grid, guess one candidate, and solve forward. If you hit a contradiction, the other
candidate was right. It always works and it's how computers brute-force, but it's
unsatisfying by hand and error-prone, so treat it as the move of last resort.

## The honest takeaway

For human enjoyment, Lessons 1 to 8 solve the vast majority of puzzles. Lessons 9 to
11 cover almost all of the rest. Lesson 12 is for the few diabolicals and for the fun
of it. Don't feel you must master the deep end to be good.
