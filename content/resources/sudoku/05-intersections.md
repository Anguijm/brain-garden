---
type: lesson
series: sudoku
lesson: 5
title: Intersections — pointing and claiming (locked candidates)
status: curated
created: 2026-06-16
---

# Lesson 5 — Intersections: pointing and claiming

These are about the overlap between a box and a line (a row or column). A box and a
line share exactly three cells. When a digit gets trapped in that shared strip, it
leaks information from the box to the line, or from the line to the box. Both moves
are also called **locked candidates**.

## Pointing (box → line)

Look inside one box at a single digit. If every place that digit can still go in the
box sits in the *same row* (or the same column), then the digit is locked to that
line within the box. It must end up on that line, so **erase that digit from the
rest of that row (or column) outside the box.**

Plain version: "In this box, the 5 can only be in these two cells, and they're both
in row 4. So the 5 in this box is somewhere on row 4. That means no 5 anywhere else
on row 4."

## Claiming (line → box), also called box-line reduction

Now flip it. Look at one digit in a single row (or column). If every place that
digit can still go in that row sits inside the *same box*, then the digit is locked
to that box. It must end up in that box, so **erase that digit from the rest of that
box** (the cells of the box that are on other rows/columns).

Plain version: "On row 4, the 5 can only go in cells that all happen to live in the
left box. So row 4's 5 is in the left box. That means no 5 anywhere else in the left
box."

## How to spot them

After basic singles stall, sweep each box digit by digit and ask "are all my spots
for this digit on one line?" (pointing). Then sweep each line and ask "are all my
spots for this digit in one box?" (claiming). These two are the most reliable
"unstickers" once singles and subsets dry up.
