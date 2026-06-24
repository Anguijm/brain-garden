---
type: lesson
series: navisworks-freedom-2026
chapter: 9
title: Glossary and file types
status: curated
tags: [software, autodesk, navisworks, bim, 3d-viewer, tutorial, glossary]
created: 2026-06-24
---

# Chapter 9 — Glossary and file types

## File types

- **NWD** (`.nwd`) — Navisworks Document. A published, self-contained snapshot:
  all geometry plus review data (viewpoints, comments, clash results, 4D). The
  format you open in Freedom.
- **NWC** (`.nwc`) — Navisworks Cache. A cached copy of one source CAD file's
  geometry/data; the bridge from native CAD into Navisworks. **Freedom can't open
  it.**
- **NWF** (`.nwf`) — Navisworks File Set. The author's master file; holds pointers
  to source files plus coordination data, but no geometry of its own. **Freedom
  can't open it.**
- **DWF / DWFx** (`.dwf` / `.dwfx`) — Autodesk Design Web Format; a lighter
  shareable 3D format Freedom can open (carries geometry and properties, but not
  all the dynamic review data an NWD does).
- **RCS / RCP** (`.rcs` / `.rcp`) — Autodesk ReCap point-cloud (scan) files;
  Freedom can open these too.

## Editions

- **Navisworks Freedom** — the free, read-only viewer (this course).
- **Navisworks Simulate** — paid; adds model aggregation, object animation, 4D
  `TimeLiner`, and `Quantification`. No clash detection.
- **Navisworks Manage** — paid; everything in Simulate plus `Clash Detective`.

## Terms

- **Scene View** — the 3D area where you view and navigate the model.
- **Selection Tree** — the model's object hierarchy.
- **Properties** — the data attached to a selected object.
- **Saved Viewpoint** — a camera view (and its state) the author saved into the
  file; you recall them in Freedom but can't create them.
- **Redline / markup** — annotations drawn over a viewpoint (view-only in Freedom).
- **Comment / tag** — text notes attached to viewpoints or clashes (view-only).
- **Clash / Clash Detective** — interference between elements; detection is a
  Manage feature, Freedom only views saved results.
- **Sectioning** — cutting the model with planes or a box to see inside
  (temporary in Freedom).
- **Measure** — temporary distance/angle/area readouts.
- **TimeLiner** — the 4D tool linking objects to a schedule; Freedom plays back a
  sequence but can't author it.
- **Animator / Scripter** — the authoring tools for object animation; playback
  only in Freedom.
- **ViewCube / SteeringWheels / Navigation Bar** — the on-screen navigation aids.
- **Realism** — Collision, Gravity, Auto Crouch, Third Person; make Walk/Fly behave
  physically.
- **Publish** — the Manage/Simulate command that bakes an NWF into a shareable NWD;
  the step that produces the files Freedom opens.

Back to the [course overview](index.md).
