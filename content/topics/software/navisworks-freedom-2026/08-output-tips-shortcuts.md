---
type: lesson
series: navisworks-freedom-2026
chapter: 8
title: Output, performance, and shortcuts
status: curated
tags: [software, autodesk, navisworks, bim, 3d-viewer, tutorial]
created: 2026-06-24
---

# Chapter 8 — Output, performance, and shortcuts

## Getting something out of Freedom

Freedom is read-only, but you can still get a picture out of it.

FACT:

- **Export an image** of the current view via the **Output** tab (`Visuals`
  panel) or the application menu, choose Bitmap, JPEG, or PNG and a size, then save.
- **Print** the current view with `Ctrl+P`.

FACT, what you can't do: there's no **Export NWD** (that's a full-Navisworks
command), and you can't export viewpoints, comments, or reports to separate files.
A screenshot and a printout are the reliable outputs. To capture a walkthrough as
video you'd screen-record outside Freedom.

## Performance with big models

Assessment, with FACT where noted:

- FACT: An NWD's size and speed are mostly set **upstream**, the author optimizes
  the model before publishing. Freedom just views what it's given.
- Use a **section box** ([chapter 6](06-sectioning-and-measuring.md)) to limit the
  working volume to the area you're reviewing; it cuts what the graphics card has
  to draw and smooths navigation.
- Drop the render style from Full Render to **Shaded**
  ([chapter 3](03-navigation.md)) if navigation stutters.
- Keep your graphics drivers current; weak or out-of-date drivers hurt large-model
  performance most.
- FACT: because an NWD is self-contained (no external links), it won't suffer the
  broken-reference problems an NWF can; if a file won't open, it's usually the
  wrong format (NWC/NWF/native CAD) or a corrupt download.

## Handy keyboard shortcuts

FACT (Navisworks default shortcuts; note Freedom's shortcuts are **not**
customizable):

Navigation tools:
- `Ctrl+1` Select · `Ctrl+2` Walk · `Ctrl+3` Look Around · `Ctrl+4` Zoom ·
  `Ctrl+5` Zoom Window · `Ctrl+6` Pan · `Ctrl+7` Orbit · `Ctrl+8` Free Orbit
  (Examine) · `Ctrl+9` Fly · `Ctrl+0` Turntable

View and realism:
- `Home` home view · `PgDn` zoom to selected · `Esc` deselect · `F11` full screen
- `Ctrl+D` Collision · `Ctrl+G` Gravity · `Ctrl+T` Third Person

Windows and help:
- `Ctrl+F3` TimeLiner · `Ctrl+H` Unhide All · `F1` Help · `F12` Options
- (Saved Viewpoints, Selection Tree, Properties, and Comments each have a default
  function-key shortcut too; if one doesn't match your build, open it from the
  **View** tab instead.)

Other: press `Alt` to show the ribbon's KeyTips (letters for keyboard access);
middle-mouse drag pans, the scroll wheel zooms.

## Read-only, restated, and how to get more

Assessment: keep the one rule in mind, **Freedom shows you everything in the NWD
and lets you look around, but it saves nothing.** Measurements, sections, hides,
and markups all vanish on close. The moment you need to *author*, create and save
viewpoints, combine models, run clash detection, build a 4D schedule, you've
outgrown Freedom and want Navisworks Simulate (authoring + 4D) or Manage (adds
clash detection). Say the word and I'll write that course.

Next: [Glossary and file types](09-glossary.md).
