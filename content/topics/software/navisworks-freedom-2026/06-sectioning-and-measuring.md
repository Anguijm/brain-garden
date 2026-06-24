---
type: lesson
series: navisworks-freedom-2026
chapter: 6
title: Sectioning and measuring
status: curated
tags: [software, autodesk, navisworks, bim, 3d-viewer, tutorial]
created: 2026-06-24
---

# Chapter 6 — Sectioning and measuring

Freedom can slice the model open and take measurements. Both are genuinely useful
for review, with one constant caveat: in Freedom they are **temporary**, they live
only in the current session and cannot be saved.

## Sectioning (cutting the model open)

FACT: On the **Viewpoint** tab, `Sectioning` panel, turn on **Enable Sectioning**.
A contextual **Sectioning Tools** tab appears. Two modes:

- **Planes** — up to six cutting planes in any orientation; by default a plane is
  created through the middle of the model.
- **Box** — show only the geometry inside a section box; everything outside is
  hidden.

FACT: On the Sectioning Tools tab, the `Transform` controls (Move, Rotate, Scale
gizmos, or typed values) position and size the planes or box. **Fit Selection**
snaps the section to the bounds of whatever you've selected.

Assessment: a section box is the best way to focus on one room or level, draw the
box around the area, and the rest of the building disappears so you can navigate
the part you care about. Heads-up: if you switch to Box mode and the box ends up
outside the geometry, the model can appear to vanish; just reset or re-fit the box.

## Measuring

FACT: On the **Review** tab, `Measure` panel:

- **Point to Point** — straight-line distance between two points.
- **Point to Multiple Points** — distances from one point to several.
- **Point Line / Accumulate** — running totals along a path.
- **Angle** — the angle between two lines.
- **Area** — an area you outline.
- **Shortest Distance** — the closest gap between two selected objects (great for
  checking clearances).

FACT: Click points on the model to register them (clicking empty background
registers nothing; right-click restarts). The **Measure Tools** window shows the
start and end X/Y/Z, the difference per axis, and the total distance; for
Accumulate it shows the running total. Distance labels and an area value are drawn
right on the model.

FACT: **Convert to Markup** turns a measurement into a redline annotation. Useful
to know, but remember it (and the measurement itself) is temporary in Freedom and
cannot be saved into the file. `Clear` removes all measurement lines from the view.

Assessment: **Shortest Distance** is the underrated one for review, select a pipe
and a beam and it tells you the real clearance between them, which is exactly the
question coordination meetings keep asking.

Next: [Playing back 4D simulations and animations](07-simulation-playback.md).
