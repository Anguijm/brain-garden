---
type: lesson
series: sudoku
lesson: 10
title: Chains — remote pairs and XY-chains
status: curated
created: 2026-06-16
---

# Lesson 10 — Remote pairs and XY-chains

Colouring chained one digit. These chain through bivalue cells (two-candidate cells)
and let the logic hop between digits.

## Remote pairs

A run of bivalue cells that all share the **same two candidates**, say {4,8}, each
link seeing the next. Because each cell is a 4/8 flip, the values alternate down the
chain: if the first is 4 the next is 8, then 4, then 8. So the two ends of an
even-length chain are *opposite* (one is 4, the other is 8) and in fact between them
they cover both digits. **Any cell that sees both ends of the chain loses both 4 and
8.** Think of it as a naked pair stretched across the board.

## XY-chains

The general version. A chain of bivalue cells linked so that each shared border uses
a common digit: {A,B} - {B,C} - {C,D} - ... Each link is an on/off switch through the
shared digit. Build the chain so that both **ends share the same candidate Z** (the
first cell can be Z, the last cell can be Z). The chain guarantees that at least one
end is Z. So **any cell that sees both ends loses Z.**

## Plain version

"Start at a cell that's Z or W. If it's not Z, follow the switches: that forces the
next cell, which forces the next, and the far end comes out Z. So one end or the
other is Z. Anything that can see both ends can't be Z."

## How to spot them

Map your bivalue cells, they're the raw material. For remote pairs, look for several
cells with the identical pair, connected in a line of sight. For XY-chains, walk from
one bivalue cell to another that shares a candidate, trying to start and finish on
the same digit. These reward practice; start with short chains (3 to 5 cells).
