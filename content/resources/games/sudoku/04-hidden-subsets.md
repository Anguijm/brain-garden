---
type: lesson
series: sudoku
lesson: 4
title: Hidden subsets — pairs, triples, quads
status: curated
tags: [sudoku, games, puzzles, logic, brain-training]
created: 2026-06-16
---

# Lesson 4 — Hidden subsets (pairs, triples, quads)

Naked subsets look at the cells. Hidden subsets look at the *digits*. A hidden
subset is N digits that can only land in the same N cells within a unit, even though
those cells may also list other candidates. When that happens, the other candidates
in those cells are impostors and you erase them.

## Hidden pair

Two digits in a unit that can each only go in the same two cells. Say in one row,
both 2 and 6 can only appear in cells A and B (every other empty cell in the row is
blocked from 2 and from 6). Then A and B must be the 2 and the 6, in some order. So
**erase every candidate except 2 and 6 from cells A and B.** You've turned two
cluttered cells into a clean naked pair.

## Hidden triple

Three digits confined to the same three cells in a unit. Those three cells must hold
exactly those three digits, so **strip all other candidates out of those three
cells.** The three cells may look busy with extra candidates; that's why it's
"hidden."

## Hidden quad

Four digits confined to four cells. Same move, strip the other candidates from those
four cells. Rare, but the logic is identical.

## How to spot them

Go digit by digit inside a unit and mark where each digit can still go. If you find
two digits that share the exact same two cells (and only those), that's a hidden
pair. Three digits sharing the same three cells, a hidden triple. Hidden subsets are
worth the effort because they cut clutter that no naked technique would catch, and
they often expose a naked single right after.
