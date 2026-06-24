---
type: lesson
series: navisworks-freedom-2026
chapter: 4
title: Selecting objects and reading properties
status: curated
tags: [software, autodesk, navisworks, bim, 3d-viewer, tutorial]
created: 2026-06-24
---

# Chapter 4 — Selecting objects and reading properties

A big part of model review is interrogating objects: what is this, who made it,
what are its dimensions and parameters. Freedom reads all of that.

## Selecting objects

FACT: On the **Home** tab, `Select & Search` panel:

- **Select** — click an object in the Scene View to select it; its data appears in
  the Properties window.
- **Select Box** — drag a rectangle to select everything inside it.

FACT: Selecting in the Scene View highlights the matching item in the **Selection
Tree**, and vice versa, click a tree item to select it in 3D. Holding `Shift`
while clicking in the scene cycles the **selection resolution**, stepping up or
down the hierarchy (from a single face up to the whole element or group).

## The Selection Tree

FACT: The Selection Tree is the model's hierarchy, with a drop-down to switch
modes:

- **Standard** — the full hierarchy as authored (the default).
- **Compact** — a simplified version.
- **Properties** — items grouped by a chosen property.
- **Sets** — appears only if the file contains saved selection/search sets.

FACT: The hierarchy descends roughly file > category > family > type > instance >
geometry, mirroring the source application (so a Revit model reads in Revit terms).

FACT on sets: you can **use** selection and search sets that the author saved into
the file, but you **cannot create or save your own** in Freedom. You can run a live
search (Find Items) to locate objects, but you can't persist the result as a new
set.

## Reading object properties

FACT: The **Properties** window shows the selected object's data, with a tab for
each property category (geometry/transform categories plus whatever the source
CAD/BIM attached, materials, identity, dimensions, custom parameters, and so on).
Each tab lists property names and values.

FACT: **Quick Properties** shows a small tooltip (by default the name and type)
when you hover over an object, without selecting it, handy for quickly identifying
things as you move the cursor around. Property values are shown in the units stored
in the model.

## Hiding things to see better (temporary)

FACT: On the **Home** tab, `Visibility` panel:

- **Hide** — hide the selected objects.
- **Hide Unselected** — hide everything except the selection (isolate it).
- **Unhide All** (`Ctrl+H`) — bring everything back.

FACT: These are **temporary**, a view aid for the current session. You can't save
them into the read-only NWD, and recalling a saved viewpoint resets visibility to
whatever that viewpoint stored. Note too that **per-object color/transparency
overrides cannot be authored in Freedom**; you can only see overrides the author
already baked in.

Assessment: "hide unselected" is the fastest way to study one assembly, select the
thing you care about, isolate it, orbit around it, then Unhide All when done.

Next: [Saved viewpoints and review data](05-viewpoints-and-review.md).
