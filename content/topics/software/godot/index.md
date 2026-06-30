---
type: lesson
series: godot
chapter: 0
title: Godot, a plain-English course
status: curated
tags: [software, godot, game-development, game-engine, gdscript, tutorial, beginner-friendly]
created: 2026-06-30
---

# Godot, a plain-English course

This course explains the Godot game engine from the ground up, in plain language, for
anyone who wants to understand how it works and start making games without needing a
programming background going in. Each chapter introduces its terms once and then builds on
the one before it, so the ideas accumulate rather than piling up all at once.

A quick note on trust, the same as elsewhere in this garden: claims are labeled FACT
(checked against the official Godot documentation), Assessment (my own judgment), or
Speculation (a guess about the future).

## What a game engine is

Before Godot itself, the bigger idea. Assessment: a game engine is the reusable software
toolkit that handles the common machinery every game needs, drawing graphics on screen,
running physics, reading the keyboard and mouse, playing sound, and keeping track of all the
game's parts. Because you build your game on top of that toolkit instead of writing every
piece of it yourself from scratch, a small team can produce a real, finished game in a
reasonable amount of time.

## What Godot is

FACT: Godot is a free, open-source engine for making both 2D and 3D games, built to handle
all sorts of projects. (Godot docs, *Introduction to Godot*.)

A few things make it stand out. FACT: it is released under the MIT license, which means it
is genuinely free with "no strings attached, no royalties, nothing," so the games you make
are entirely yours. It is run by the non-profit Godot Foundation together with a community
of volunteers. (Godot docs, *About*.) Assessment: that "no royalties" point is the big
practical contrast with some commercial engines, which can take a cut of your sales or
charge per-seat fees. Godot is also lightweight, the whole editor is a single download of a
few tens of megabytes.

FACT: the current version is Godot 4.7, released in June 2026. (godotengine.org/download.)
One thing to know before you start: Godot 4.0, released in 2023, was a ground-up rewrite of
the engine, so a lot changed between the old 3.x line and today's 4.x line. (godotengine.org,
*Godot 4.0 sets sail*.) Assessment: this matters because older tutorials written for Godot 3
often will not work step-for-step in Godot 4, since many pieces were renamed. When you look
for help, prefer material made for Godot 4.

## How this course is laid out

You can read it straight through, or jump to what you need.

1. **[Nodes and scenes](01-nodes-and-scenes)** — the single most important idea in Godot,
   the mental model everything else rests on. Start here.
2. **[The editor and your first project](02-the-editor)** — a tour of the program's
   screen, and how you put a scene together.
3. **[GDScript](03-gdscript)** — Godot's built-in language, and how a script brings a node
   to life.
4. **[Making it interactive](04-interactivity)** — the game loop, reading player input, and
   signals (how parts of your game talk to each other).
5. **[Building a small game](05-first-game)** — a worked example that ties the pieces
   together.
6. **[Going further](06-going-further)** — 2D versus 3D, physics, menus, sound, and how to
   export your finished game so other people can play it.

## How to actually learn it

Assessment: reading explains the ideas, but a game engine is a hands-on tool, so the real
learning happens with the editor open in front of you and something half-built on the
screen. The best companion to this course is the official, free, step-by-step tutorial that
walks you through building a small game called "Dodge the Creeps." FACT: that tutorial, "Your first 2D game," is part of Godot's
official Getting Started docs. (Godot docs.) Read a chapter here for the why, then build
alongside the official tutorial for the how.

## Sources

- Godot Engine documentation — https://docs.godotengine.org/en/stable/
- Introduction to Godot — https://docs.godotengine.org/en/stable/getting_started/introduction/introduction_to_godot.html
- About Godot (license, Foundation) — https://docs.godotengine.org/en/stable/about/introduction.html
- Download (current version) — https://godotengine.org/download/
