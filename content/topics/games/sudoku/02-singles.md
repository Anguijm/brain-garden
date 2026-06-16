---
type: lesson
series: sudoku
lesson: 2
title: Singles — naked and hidden
status: curated
tags: [sudoku, games, puzzles, logic, brain-training]
created: 2026-06-16
---

# Lesson 2 — Singles: the two moves you'll use most

These are the two most basic solving moves, and they are literally the two
questions from Lesson 1 turned into actions.

## Naked single (answers "what's the only digit for this cell?")

A **naked single** is a cell that has only one candidate left. The other eight
digits are all banned from it because they already appear somewhere in its row,
its column, or its box.

How to spot it: pick an empty cell and ask "which digits are already visible in my
row, my column, or my box?" Cross those off. If exactly one digit survives, that's
the answer. It's "naked" because the lone candidate is sitting there in the open
once you clear the rest.

## Hidden single (answers "where's the only place this digit can go?")

A **hidden single** flips the question. Instead of staring at one cell, you stare
at one *unit* (a row, column, or box) and one *digit*, and ask: "of the empty
cells in this unit, how many can actually hold this digit?" If the answer is
exactly one, that cell must be the digit, even if that cell still lists other
candidates.

It's called "hidden" because the cell's pencil marks may show several candidates,
so the answer is disguised. You only see it by reasoning about the digit across
the whole unit, not by looking at the cell alone.

Example, digit-focused: in a box, suppose 4 is missing. Three cells are empty. Two
of those empty cells sit in rows that already contain a 4 elsewhere, so 4 is banned
from them. That leaves one empty cell as the only home for 4 in the box. Place it,
even if that cell could also have taken a 7 or a 9.

## Why both matter

- **Naked single**: easy to verify, but it only appears once a cell is almost
  fully constrained. Early in a puzzle you'll find few.
- **Hidden single**: the workhorse. It finds placements much earlier, because a
  digit can be forced into a cell long before that cell is down to one candidate.

Most "I'm stuck" moments early on are missed hidden singles. Train the
digit-focused scan: pick a digit, walk all nine boxes (or rows, or columns), and
ask where it can still go.
