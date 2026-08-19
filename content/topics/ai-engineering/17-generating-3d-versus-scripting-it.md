---
type: lesson
series: ai-engineering
chapter: 17
title: Generating 3D, and why you cannot just script it
status: curated
tags: [ai, 3d-printing, generative-models, diffusion, code-generation, making, using-ai-well]
created: 2026-08-18
---

# Why an AI can sculpt a goblin but cannot write the code for one

Here is a fair question. Tools like Meshy and Hyper3D's Rodin will turn a sentence into a
3D model you can print. You also have Blender and OpenSCAD sitting on your own computer,
and an AI that writes code all day. So why not just ask it to write the script that makes
the miniature?

The instinct that there is a real difference here is right. The usual way of putting it,
that describing a picture in words is not the same as making one, is close but not quite
the thing. The sharper version is this: **one machine writes instructions for building a
shape, and the other produces the shape itself.** Those are different jobs, they fail in
opposite ways, and a Dungeons-and-Dragons miniature sits on the far side of the line from
where code is any good.

## What Meshy and Rodin are actually doing

Start with what these tools are not. They are not writing a program. They are not
assembling primitives. They are sampling from a learned space of three-dimensional shapes,
the same way an image generator samples from a learned space of pictures.

Rodin's underlying method is published, which makes it a good place to look. FACT: the
CLAY paper, *CLAY: A Controllable Large-scale Generative Model for Creating High-quality
3D Assets* by Longwen Zhang and co-authors, posted to arXiv on 30 May 2024, describes a
model that "adopts neural fields to represent continuous and complete surfaces" and is
"composed of a multi-resolution Variational Autoencoder (VAE) and a minimalistic latent
Diffusion Transformer (DiT), to extract rich 3D priors" from a large 3D dataset. FACT: the
same paper describes the result as "a 3D native geometry generator with 1.5 billion
parameters."

Three pieces of jargon there, each worth a plain sentence.

A **neural field** means the shape is not stored as a list of triangles. It is stored as a
function: hand it any point in space and it tells you whether that point is inside the
object or outside it, and how far from the surface. The surface is wherever that answer
crosses zero. Nothing is made of polygons yet; the shape is a continuous mathematical
object.

A **variational autoencoder** is a squeezer. It learns to compress thousands of shapes down
to a short list of numbers and then rebuild them. What matters is what happens in the
squeezed middle: similar shapes end up near each other, so the compressed space becomes a
map of shape-ness where you can move around and land on plausible new objects.

A **diffusion transformer** is the part that generates. Diffusion models learn by watching
things dissolve into noise and practising the reverse, so they can start from pure noise
and walk it back into something coherent, steered by your text prompt.

Assessment: put together, "3D native" is the load-bearing phrase. An earlier generation of
these tools worked indirectly, generating pictures of an object from several angles and
then reconstructing a solid from those pictures, which is a photogrammetry-flavoured
detour. A 3D-native model skips the pictures and generates in shape space directly. That is
why the output holds together as an object instead of looking correct from the angles that
were drawn and wrong everywhere else.

## Where the printable mesh comes from, and why it needs cleaning

A slicer cannot print a mathematical function. So the last step converts the field into an
actual surface of triangles, by marching through space, sampling inside-versus-outside, and
stitching a skin along the boundary.

Assessment: this is the root of nearly every complaint about printing AI-generated models.
The mesh is not designed, it is **extracted**. Its topology, meaning how the triangles
connect to each other, is a by-product of that extraction rather than a decision anyone
made. Nothing in the process is checking the things a printer cares about.

What that produces, in practice: surfaces with holes, so the model is not watertight and
the slicer cannot tell inside from outside. Non-manifold edges, which describe a solid that
cannot physically exist. Little disconnected islands of geometry left over from noise.
Normals, the flags saying which way a face points, flipped inward. Walls thinner than the
printer can produce.

FACT: Tripo's practitioner guide lists "non-manifold edges (where more than two faces
meet), self-intersections, and internal geometry," and adds that "you'll also see tiny,
disconnected 'island' meshes from noise in the generation process and flipped normals"
([Tripo](https://www.tripo3d.ai/blog/explore/ai-3d-model-generator-and-manifold-repair-automation)).
FACT: Meshy's guide states that "AI-generated models produce these errors more often than
CAD models because neural-network geometry optimizes for visual appearance, not topological
correctness," and that slicers "require a fully closed, watertight mesh"
([Meshy](https://www.meshy.ai/blog/fix-non-manifold-edges-stl-repair)).

That second sentence is worth pausing on, because a vendor is conceding the argument of
this whole chapter: the geometry is an output of something optimising for how the object
looks, and topological correctness was never what it was solving for. Tripo says the same
thing from the other side, that the process is "statistical, not procedural, meaning the
model's underlying topology (the mesh's wireframe structure) is an emergent property."

Assessment: both companies sell the repair step, so read the emphasis with that in mind.
The underlying geometry they describe is standard and not in dispute, and neither is an
independent study.

## The other machine: geometry as code

OpenSCAD is a programming language for solid shapes. You do not push vertices around. You
write out a construction: take a cylinder, subtract a smaller cylinder, move it twelve
millimetres left, repeat it six times around a circle. The software executes your program
and the object appears.

For a certain kind of object this is wonderful, and better than sculpting. A bracket, a
knob, a gear, a jig, a battery tray. Anything where the dimensions are the point, where you
want to change one number and have everything else follow, and where the finished thing can
honestly be described as a handful of boxes and cylinders combined.

A goblin is not that. Neither is a cloak, a face, a hand, or the way muscle sits over bone.

## How badly it actually goes, measured

This has been benchmarked, which beats guessing.

FACT: *P3D-Bench: Benchmarking MLLMs for Parametric 3D Generation and Structural
Reasoning* (arXiv, 9 June 2026) tests large language models on producing executable
parametric 3D code, including OpenSCAD, from text, images and annotated assemblies. FACT:
its abstract states that "the strongest MLLM already aligns well with the input
semantically (*J-Sem* 0.8), but its geometry matches the specified dimensions far less
accurately (*J-Geo* 0.35)." Those are the benchmark's judge scores on a 0-to-1 scale, not
the share of attempts that succeed.

Read that twice, because it is the whole answer in one sentence. The model reliably
produces something that **is recognisably the right kind of object**. It much less reliably
produces something **with the right dimensions in the right places**. Knowing what to build
and building it precisely are separate skills, and current models are far better at the
first.

Assessment: notice that this is the exact opposite of how the generative 3D tools fail. A
diffusion model will hand you a beautiful goblin whose axe is 30% too large and whose base
is not 25mm across, because it was never reasoning about millimetres at all. A code model
will hand you something dimensioned to the tenth of a millimetre that does not look like a
goblin. **They are each strong precisely where the other is weak**, which is the most
useful thing to hold on to.

## So what is the real difference?

Your instinct was that it resembles the gap between describing a picture and drawing one.
Here is the sharper version, in three parts.

**One is blind, and the other is not.** A model writing OpenSCAD emits text. The geometry
does not exist until the code runs, which happens after the writing is finished. It is
choosing coordinates without ever seeing the result, like describing a sculpture over the
phone in numbers to someone who will carve it while you are hung up. A generative 3D model
works directly in the space of shapes, so there is no gap between what it decides and what
appears.

**The language itself is a wall.** Code-based modelling means combining primitives with
union, difference and intersection. That vocabulary spans mechanical parts beautifully and
organic form terribly. Even researchers building agents to drive Blender say so plainly.
FACT: the Planner-Actor-Critic paper (arXiv, 8 January 2026) lists among its limitations
that "the code execution approach often rely on modeling with primitive, which limit
expressiveness."

**One learned from shapes, the other learned from text.** A 3D-native generator was trained
on an enormous pile of actual three-dimensional objects, so it has absorbed how a shoulder
meets an arm. A language model learned to produce OpenSCAD from source code and
documentation. It knows what the language looks like. It has far less sense of what the
code will *look like when run*.

## But what if you let it look?

The obvious fix is a loop: write the script, render a picture, let the model look at the
picture, criticise, revise. This exists, and it helps.

FACT: *From Idea to Co-Creation: A Planner-Actor-Critic Framework for Agent Augmented 3D
Modeling* by Jin Gao and Saichandu Juluri (arXiv, 8 January 2026) splits the work into a
Planner that keeps a task list, an Actor that runs Blender operations and Python, and a
Critic that reads viewport screenshots and writes structured feedback.

FACT: the same paper reports that "the actor does not always incorporate the critic's
feedback, and after multiple iterations (often three), the modeling quality degrades or
converge, making further improvements difficult."

Assessment: about three useful rounds, then it stalls. That is enough to nudge a shape and
nowhere near enough to sculpt a face. The loop closes the blindness problem partly, and
does nothing at all about the vocabulary problem, which is the one that actually blocks
miniatures.

## What this means if you want to print the thing

A few practical consequences, because "which tool" is downstream of "what are you printing
on."

**Resin and filament are not the same conversation.** FACT: AmeraLabs tested layer heights
from 10 to 100 microns on miniatures and concluded that 50 microns is "ideal for most minis
and surfaces," while 20 or 10 microns "reached display quality" on unpainted pieces
([AmeraLabs](https://ameralabs.com/blog/layer-height-for-miniatures/)). FACT: recommended
wall thickness for resin miniatures is "around 1.2mm and above"
([3D Printerly](https://3dprinterly.com/best-wall-thickness-for-resin-3d-prints-miniatures-more/)).
FACT: 28mm "refers to the height of the miniature from the base to the eyes or top of the
head" and is "typically associated with the 1:56 scale"
([AmeraLabs](https://ameralabs.com/blog/miniature-scale-3d-printing/)).

Assessment: those layer figures are resin territory. A fused-filament machine
pushing plastic through a 0.4mm nozzle is a fine printer that is being asked the wrong
question at this scale, which is why the miniature hobby standardised on resin. With a
filament printer the sensible routes are printing larger than 28mm, or printing a master
and casting from it.

**The hybrid is the actual answer.** Generate the organic form with a 3D-native model,
because that is the half code cannot do. Then use code, or Blender by hand, for everything
dimensional: scale it so the figure is genuinely 28mm to the eye, put it on a base that is
exactly 25mm across, hollow it, add drain holes, cut it for supports. That half is
parametric, repeatable and unglamorous, which is precisely where a script shines.

**Budget for repair as a real step, not an afterthought.** The mesh arrives extracted
rather than designed, so assume it needs making watertight before it meets a slicer.

## The honest bottom line

- Meshy and Rodin generate in a learned space of shapes. They are not writing code and
  never were.
- A model writing OpenSCAD is working blind, in a vocabulary built for brackets.
- Measured on P3D-Bench, the strongest model scores 0.8 for getting the *idea* right and
  0.35 for getting the *dimensions* right. That gap is the entire story.
- The two approaches fail at opposite things, so the useful move is to use both: generated
  form, scripted dimensions.
- None of this removes the mesh-repair step, and none of it makes a 0.4mm nozzle able to
  print a face at 28mm.

## See also

- **In this series:** [← Web automation and bot defences](topics/ai-engineering/16-web-automation-and-bot-defenses) · [The plan that runs →](topics/ai-engineering/18-the-plan-that-runs) · [Overview](topics/ai-engineering/)
- **[Pictures and voice](topics/ai-engineering/05-pictures-and-voice)** — the same
  generate-versus-describe split, one dimension down.
- **[Making with the Bambu X1C](connections/making-with-the-x1c)** — the printer these
  decisions actually land on.
- **[Cold-cast game pieces](topics/making/cold-cast-game-pieces/)** — the print-a-master-
  and-cast route, which is how a filament printer gets around its own resolution limit.

## Sources

- Longwen Zhang et al., *CLAY: A Controllable Large-scale Generative Model for Creating High-quality 3D Assets* (arXiv, 30 May 2024) — https://arxiv.org/abs/2406.13897
- *P3D-Bench: Benchmarking MLLMs for Parametric 3D Generation and Structural Reasoning* (arXiv, 9 June 2026) — https://arxiv.org/html/2606.11152v1
- Jin Gao and Saichandu Juluri, *From Idea to Co-Creation: A Planner-Actor-Critic Framework for Agent Augmented 3D Modeling* (arXiv, 8 January 2026) — https://arxiv.org/html/2601.05016
- Meshy, *How to Fix Non-Manifold Edges in STL Files for 3D Printing* (vendor documentation) — https://www.meshy.ai/blog/fix-non-manifold-edges-stl-repair
- Tripo, *AI 3D Model Generation & Automated Manifold Repair* (vendor documentation) — https://www.tripo3d.ai/blog/explore/ai-3d-model-generator-and-manifold-repair-automation
- AmeraLabs, *Best Layer Height for Miniatures* — https://ameralabs.com/blog/layer-height-for-miniatures/
- AmeraLabs, *Miniature Scale for 3D Printing* — https://ameralabs.com/blog/miniature-scale-3d-printing/
- 3D Printerly, *Best Wall Thickness for Resin 3D Prints* — https://3dprinterly.com/best-wall-thickness-for-resin-3d-prints-miniatures-more/
