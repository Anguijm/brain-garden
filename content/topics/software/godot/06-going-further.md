---
type: lesson
series: godot
chapter: 6
title: Going further
status: curated
tags: [software, godot, game-development, 3d, physics, exporting, beginner-friendly]
created: 2026-06-30
---

# Going further

By now you have the whole core of Godot: [nodes and scenes](01-nodes-and-scenes),
[the editor](02-the-editor), [GDScript](03-gdscript),
[interactivity](04-interactivity), and how they [combine into a game](05-first-game). This
last chapter is a map of where to go next. It is a quick tour, not a deep dive, so you know
what exists and what to search for when you need it.

## 2D and 3D

FACT: Godot makes both 2D and 3D games, and they use the same node-and-scene model you
already know. (Godot docs, *Introduction to Godot*.) The difference is mostly the extra
dimension: 3D adds depth, so you work with 3D models (called meshes), lights, and a camera
that moves through space. As you saw in chapter 1, the node names tell you which world you
are in, 2D nodes end in `2D` and 3D nodes end in `3D` (a `Sprite2D` versus a
`MeshInstance3D`). Assessment: start with 2D. It has fewer moving parts, the concepts are
the same, and almost everything you learn carries straight over to 3D later.

## Physics: collisions and gravity

Assessment: most games need things to bump into each other, and Godot has a built-in physics
engine for that, so you do not calculate collisions yourself. The pieces you will meet:

- **Bodies** are the physical objects. You met `CharacterBody2D`, the kind you move directly
  for a player. There are also bodies the engine pushes around with gravity, and unmoving
  bodies for walls and floors.
- **Collision shapes** (the `CollisionShape2D` from chapter 1) are the simple outlines the
  engine actually checks for touching, kept separate from the picture so collision stays
  fast.
- **Areas** (`Area2D`) detect when something enters a region without blocking it, which is
  how you build coin pickups, damage zones, and trigger spots. When something enters, the
  area emits a signal, the same signals from chapter 4.

## Menus and on-screen displays

FACT: for buttons, menus, labels, and health bars, Godot has a family of UI nodes called
Control nodes. (Godot docs, *GUI*.) Assessment: their special trick is that they know how to
position and resize themselves for different screen sizes, so your menu still looks right on
a phone and a monitor. A title screen, a pause menu, and the score display from chapter 5
are all built from Control nodes.

## Sound

Assessment: sound in Godot comes from dedicated audio-player nodes. You add one, hand it a
music file or a sound effect, and tell it to play, often in response to a signal, like
playing a "coin" sound when an `Area2D` reports a pickup. Background music and one-off sound
effects work the same way, just on different player nodes.

## Sharing your game: exporting

A project only runs inside the editor until you export it. FACT: exporting turns your project
into a standalone program other people can run, and Godot can export to Windows, macOS,
Linux, the web (so it plays in a browser), Android, and iOS. (Godot docs, *Exporting
projects*.)

FACT: it works through two pieces. "Export templates" are the prebuilt engine files for each
platform, which you download once from inside the editor. "Export presets" are the settings
for a given target, which you add under the Project menu. With those in place, you pick a
preset and export. (Godot docs.) Assessment: the web export is the easiest way to share early
work, since anyone can play it from a link with nothing to install. Some platforms (phones in
particular) need extra setup, but the default options are enough to get a desktop or web
build out the door.

## Where to keep learning

Assessment: the official Godot documentation is the authority, and it is genuinely good and
free. The Getting Started section and the "Your first 2D game" tutorial (the Dodge the
Creeps project from chapter 5) are the right next stops, and the class reference explains
every node in detail when you need specifics. FACT: all of it lives at
docs.godotengine.org. (Godot docs.)

## The takeaway

Assessment: you now have the full shape of Godot. Games are trees of nodes grouped into
reusable scenes; you arrange them in the editor; scripts in GDScript give them behavior; the
game loop, input, and signals make them interactive; physics, UI, and audio fill them out;
and exporting shares them with the world. Everything beyond this course is a deeper look at
one of those pieces, built on the same foundation you already have.

## Sources

- Godot docs, *Exporting projects* — https://docs.godotengine.org/en/stable/tutorials/export/exporting_projects.html
- Godot docs, *Introduction to Godot* — https://docs.godotengine.org/en/stable/getting_started/introduction/introduction_to_godot.html
- Godot docs, *GUI / Control nodes* — https://docs.godotengine.org/en/stable/tutorials/ui/index.html
- Godot documentation (home) — https://docs.godotengine.org/en/stable/
