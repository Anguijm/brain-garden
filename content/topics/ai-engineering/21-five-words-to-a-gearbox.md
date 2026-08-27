---
type: lesson
series: ai-engineering
chapter: 21
title: Five words to a gearbox, and where the knowledge came from
status: curated
tags: [ai, openscad, code-generation, mechanical-engineering, making, verification, using-ai-well]
created: 2026-08-27
---

# Five words to a gearbox, and where the knowledge came from

[Chapter 17](topics/ai-engineering/17-generating-3d-versus-scripting-it) made the case that geometry-as-code
is the wrong tool for a goblin. This is the other half of that argument: the case where it is exactly the
right tool, where a handful of words produces something close to a real machine, and where the knowledge
that made it close actually came from.

The question that started it was blunt. Someone typed roughly *"make me an MRG for a DDG"* and got back a
gearbox with the right architecture. Not a vague gearbox. The right number of inputs, the right number of
shafts, the right count of pinions. Five words in, a correct topology out. How?

The answer is not that the model knows about destroyers. It is that those five words contain a **proper
noun**, and a proper noun is not a description. It is an index key.

## What "DDG" actually did

"Make me a gearbox" has no referent. Nothing to look up, so everything is invented.

"An MRG for a DDG" names a specific, real, extensively documented machine. That difference is worth more
than any amount of adjectives.

FACT: the Federation of American Scientists' DDG-51 page states that each engine room contains two LM2500
gas turbines and one propulsion reduction gear, converting high-speed low-torque turbine output into
low-speed high-torque output to drive the shafting. FACT: Timken announced on February 21, 2023 that it
would continue supplying main reduction gears for the DDG-51 class, describing them as drive systems that
transfer torque from the ship's gas turbines to its propeller shafts.

So the chain runs:

```
  "MRG for a DDG"
        |
        v  proper noun resolves to a documented artifact
  DDG-51 class: 4 gas turbines, 2 shafts, 2 gearboxes, 2 turbines per gearbox
        |
        v  that class of artifact uses a standard arrangement
  locked-train double-reduction gear
        |
        v  the arrangement forces the topology
  2 high-speed pinions / 4 first-reduction gears / 4 low-speed pinions / 1 bull gear
```

FACT: the description of the locked-train arrangement is published in Navy engineering training material,
which states that adjacent high-speed and low-speed intermediate assemblies are locked in mesh, giving two
first-reduction high-speed pinions, four first-reduction high-speed gears, four second-reduction low-speed
pinions and one bull gear, with each first-reduction pinion driving two first-reduction gears and each of
those connected by a quill shaft and flexible couplings to a second-reduction pinion.

The last step in that chain is the one people find surprising, and it is the least mysterious. Once you
have said *locked train* and *two inputs*, the pinion count is not recalled from anywhere. It is forced.
"Locked train" means each input splits torque into two paths that recombine at the bull gear. Two inputs,
two paths each, one bull gear: 2 / 4 / 4 / 1. There is exactly one valid graph. The arithmetic does the
work, not the memory.

## The step where knowledge actually enters

Notice what happened between the five words and the code. The model wrote a specification. Nobody asked it
to; it did it because it cannot write geometry without one.

**That expansion is where every piece of borrowed knowledge got inserted**, and it is the only part of the
process a person can read at a glance. Everything downstream is consequences.

This is the single most useful practical handle in this whole note. Before any code gets written, ask for
the expansion: *tell me what you think I asked for, and list every number you are assuming.* A wrong
assumption is obvious in a sentence and nearly invisible once it is baked into a coordinate.

## What the public record gives, and what it withholds

Rather than argue about this, it is worth testing. Searching the open literature for the DDG-51 main
reduction gear returns the architecture immediately: two turbines per gearbox, the horsepower rating, the
supplier, the locked-train arrangement, the pinion and gear counts, the quill shafts.

It returns none of the following: shaft rpm, propeller design rpm, the actual reduction ratio, tooth
counts, module or diametral pitch, face width, centre distance, helix angle, bearing sizes.

That is not a poor search. It is the real boundary. Those numbers live in vendor drawings and technical
manuals, much of it distribution-limited. **A language model does not have them either, for exactly the
same reason.** It will still produce a number if asked.

## The line that actually matters: structure versus magnitude

The useful division is not "things it knows" against "things it doesn't." It is this:

**Structure** — how many shafts, what meshes with what, what is rigidly coupled, load paths, how things
nest — is published, standardised, taught, and frequently implied by the vocabulary itself. Models are
strong here.

**Magnitude** — ratios, tooth counts, clearances, tolerances, face widths, fits — is design data. It is
precisely what does not get published, and it is where confident wrongness lives.

This is not a hunch. It has been measured. FACT: *P3D-Bench: Benchmarking MLLMs for Parametric 3D
Generation and Structural Reasoning* (arXiv, June 9, 2026) reports that the strongest model tested "already
aligns well with the input semantically (*J-Sem* 0.8), but its geometry matches the specified dimensions
far less accurately (*J-Geo* 0.35)," on a zero-to-one judge scale.

Assessment: 0.80 for *is this the right kind of object* against 0.35 for *are the dimensions right* is the
structure-magnitude split with numbers on it. Knowing what to build and building it to size are separate
skills, and the gap between them is large enough to plan around.

## How the math actually works once you are in OpenSCAD

For a mechanical engineer this part is more familiar than it sounds, because it is the way you were taught
to dimension a drawing, not the way you were taught to draw one.

**It is not a mesh. It is a construction tree.** OpenSCAD stores the *operations*, not the surface: take a
cylinder, subtract a smaller one, rotate it, repeat it around a bolt circle. Union, difference,
intersection, applied to primitives. The solid is defined exactly, in real numbers, and only becomes
triangles at the last moment when an STL is exported.

That is the whole reason a printed OpenSCAD part is dimensionally correct while an AI-generated mesh is
not. One is computed; the other is estimated from a picture.

**Dimensions are derived, not typed.** The numbers live at the top as named variables, and every geometric
quantity is algebra on those variables. A gear pair is the clean example:

```
module_mm      = 8;                       // tooth size
z_pinion       = 21;                      // teeth
ratio          = 4.6;
z_gear         = round(z_pinion * ratio);
d_pinion       = module_mm * z_pinion;    // pitch circle diameter
d_gear         = module_mm * z_gear;
centre_dist    = (d_pinion + d_gear) / 2;
```

Change `module_mm` and the pitch diameters, the centre distance and the housing bore all move together,
because they were never independent numbers. They were expressions.

**Constraints do most of the design work, which is why it lands close.** Pitch diameter is module times
tooth count. Centre distance is half the sum of the pitch diameters. So fixing the module and the centre
distance fixes the *sum* of the tooth counts. Fix the ratio as well and both counts are determined, up to
rounding to an integer. There are not many valid answers.

That is the honest reason a model can produce a plausible reduction gear without ever having seen the real
one. **Gear design is over-constrained.** Give it input speed, output speed and a standard arrangement and
the remaining freedom is small. It is not recalling a gearbox; it is solving a narrow problem the same way
a designer would, and conventions like choosing hunting ratios — tooth counts sharing no common factor, so
every pinion tooth eventually meets every gear tooth and wear distributes evenly — are learned practice
sitting on top.

**Curves become facets only at the end.** `$fn`, `$fa` and `$fs` decide how many flats a cylinder is
rendered with. Everything upstream is exact; this is the single moment where exactness becomes
approximation, and it is where print quality lives.

**Watertight by construction, with one classic trap.** Boolean operations on closed solids produce closed
solids, so the output is manifold without the repair pass that image-derived meshes need. The trap is
coincident faces: subtract a hole exactly as deep as the wall is thick and the two surfaces land in the
same plane, which can produce zero-thickness artefacts. The standard fix is to make the cutting tool
slightly longer than the thing it cuts.

**And an involute tooth is generated, not drawn.** You do not sketch the flank. You evaluate the involute
equation, sweep it, and subtract it around the blank. The profile is correct because it was computed from
the definition of the curve.

## Why it is fast

Because the model is blind. Chapter 17 makes this point and it holds: a model writing OpenSCAD emits text
and never sees the result, choosing coordinates over the phone for someone else to carve.

The answer to blindness is not better prompting. It is that **a parametric design which is wrong is usually
one variable away from right.** You are not redrawing anything. You change `clearance = 0.4` to `0.6`, run
it again, and look. The loop is seconds, and the geometry was never hand-placed, so nothing else breaks.

## How to direct it so it lands close

1. **Name real things rather than describing them.** "A holder for my calipers" invents dimensions. "A
   holder for Mitutoyo 500-196-30 calipers" retrieves them. Same effort, different information content,
   because the second is a lookup key into a catalogue that has been read.
2. **Give one measured anchor.** One dimension you actually took with a rule kills most of the error,
   because the rest scales off it.
3. **Say what it does, not what it looks like.** Function implies dimensions. Adjectives do not.
4. **Name the machine it prints on.** Nozzle diameter and material change wall thicknesses and overhang
   choices.
5. **State what must not happen.** Constraints carry more information than wishes.
6. **Demand parameters at the top**, so tuning is editing a number rather than re-prompting.
7. **Read the expansion before the code.**

## Where it fails, and why you will not notice

Index keys degrade silently, and the tone never changes:

| What you name | What comes back |
|---|---|
| A DDG main reduction gear | Architecture, heavily documented, reliable |
| A gearbox for a class still in design | Invented, delivered identically |
| A specific 1974 machine in your shop | Class-level knowledge, nothing about that machine |
| "The bracket Dave made last year" | No referent at all |

The underlying rule is almost embarrassingly simple. **Accuracy tracks how often a thing has been written
down** — not how technical it is, not how complex. The same gearbox has thousands of documents describing
its architecture and a handful of controlled drawings carrying its tooth counts. One object, opposite
reliability, and nothing in the output marks the transition.

To be precise about the mechanism: nothing is being looked up at the moment you ask. Facts repeated across
enough documents become reliably reproducible; facts appearing in three controlled drawings do not.
Documentation frequency is the confidence signal, and it is invisible from the answer.

## The check that catches all of this

Print the assumptions before the geometry, then measure the first part off the plate before making six.

Which is first-article inspection, arrived at from the other direction. Prove the first unit off a process
matches the specification, then trust the process. The reason it is the right control here is the same
reason it is the right control in a shop: the failure you are guarding against is not random error, it is a
systematic one introduced upstream and faithfully reproduced every time after.

## See also

- [Generating 3D, and why you cannot just script it](topics/ai-engineering/17-generating-3d-versus-scripting-it) —
  the opposite case: why code is the wrong tool for organic form, and how the two approaches fail in
  mirror image.
- [Three image-to-3D models against a real slicer](topics/ai-engineering/20-three-image-to-3d-models-against-a-real-slicer) —
  what the mesh generators actually produce when you try to print it.

## Sources

- [DDG-51 Arleigh Burke class, Federation of American Scientists](https://man.fas.org/dod-101/sys/ship/ddg-51.htm)
- [Timken to continue providing main reduction gears for DDG-51, Feb 21, 2023](https://news.timken.com/2023-02-21-Timken-to-Continue-to-Provide-Main-Reduction-Gears-for-U-S-Navy-Arleigh-Burke-DDG-51-Class-Ships)
- [How Reduction Gears Work, Chapter 7, Gene Slover's US Navy Pages](https://www.eugeneleeslover.com/ENGINEERING/CHAPTER-7.php)
- [Reduction Gears, Massachusetts Maritime TSPS Engineering Manual](https://weh.maritime.edu/campus/TSPS/manual/DriveTrain.html)
- [P3D-Bench: Benchmarking MLLMs for Parametric 3D Generation and Structural Reasoning, arXiv, Jun 9, 2026](https://arxiv.org/html/2606.11152v1)
