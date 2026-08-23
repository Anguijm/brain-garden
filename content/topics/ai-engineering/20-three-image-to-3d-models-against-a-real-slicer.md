---
title: Three image-to-3D models, judged by the slicer instead of a ruler
tags: [ai-engineering, 3d-printing, mini-forge, rocm]
---

# Three image-to-3D models, judged by the slicer instead of a ruler

**Assessment.** TRELLIS.2, Hunyuan3D-2mv and Pixal3D were run on the same two input images
on one machine (Radeon 8060S, gfx1151), prepared identically, and then put through the actual
Bambu Studio CLI with a real X1C profile. The headline is that a mesh-thickness metric which
had been driving decisions turned out to be a poor gate, and the slicer disagreed with it.

## What was measured

Prep for every mesh: `prepare_mini.py --height-mm 32`, then sliced with
`Bambu Lab X1 Carbon 0.4 nozzle` + `0.08mm Extra Fine @BBL X1C` + `Bambu PLA Basic`.

**FACT.** Slice results, 2026-08-22:

| subject | model | prep voxel | faces under 0.80mm | slicer | print |
|---|---|---|---|---|---|
| goblin | TRELLIS.2 | 0.10 | 17.12% | **clean** | 10m55s |
| goblin | Hunyuan3D-2mv | 0.10 | 8.50% | **clean** | 9m25s |
| goblin | Pixal3D | 0.10 | 51.47% | rejected | — |
| goblin | Pixal3D | 0.20 | 7.91% | **clean** | 9m36s |
| spider | TRELLIS.2 | 0.10 | 0.00% | **clean** | 13m28s |
| spider | Hunyuan3D-2mv | 0.10 | 0.02% | **clean** | 13m30s |
| spider | Pixal3D | 0.10 | 35.35% | rejected | — |

**FACT.** On every mesh it accepted, Bambu Studio emitted **no** warning about
non-manifold geometry, self-intersection, degenerate triangles or thin walls.

## The metric was wrong, and here is why

**FACT.** The "under 0.80mm" figure counts material thinner than two extrusion widths. The
X1C lays a ~0.42mm line with a 0.4mm nozzle, and Bambu Studio prints single-extrusion
features well below 0.8mm.

**Assessment.** Two things make the figure misleading rather than merely conservative. The
first is that `prepare_mini.py` finishes with a voxel remesh, so every output is watertight by
construction; a thin region in a closed solid is not a hole, it is a place the slicer will lay
one wall instead of two. The second is that the threshold was being read as pass/fail when it
is a rule of thumb about perimeter counts. The goblin at 17.12% sliced clean and prints in
under eleven minutes.

**Assessment.** The number is still worth keeping, but as a *comparative* signal between
models on the same subject, not as a gate. Where it earns its keep is spotting a defect: a
single cluster covering the whole figure is not fine detail, it is a broken mesh.

## Pixal3D emits an open shell, and that is the real problem

**FACT.** `prepare_mini.py` reports on the raw Pixal3D GLB:
`503,463 open edges, 0 non-manifold, 43,089 component(s) -> NOT watertight`.

**FACT.** The sub-0.30mm material on the Pixal3D spider-man was a **single** cluster of 10,364
faces spanning x -8.03..8.03, z 2.09..18.08, which is the entire figure: two surfaces about
0.002mm apart over the whole body.

**FACT.** Bambu Studio rejects the fine-voxel Pixal3D meshes at load time with
`total 1 models, 0 objects` — it parses the file and discards the object. The STL itself is
sound: 691,532 triangles, all coordinates finite, zero degenerate faces, bbox
33.49 x 25.00 x 31.93mm sitting on z=0.

**FACT.** It is not a triangle-count limit. TRELLIS at 564,348 triangles slices; Pixal3D at
235,524 does not. Pixal3D at 127,088 (voxel 0.20) does.

**Assessment.** The mesh is double-layered. A coarse 0.20mm voxel remesh fuses the two
coincident surfaces into one solid and the result slices normally; finer voxels preserve them
as separate sheets and the slicer refuses the object. This is a known failure mode in this
model family: the `visualbruno/CuMesh` fork carries a commit "Added a new function
`reconstruct_mesh_dc` -> no more 'double layer'", which is untried here.

**So Pixal3D is usable today** by prepping at `--voxel-mm 0.20` instead of 0.10. At that
setting it measures 7.91% under 0.80mm, which is better than Hunyuan3D's 8.50%.

## Which model to reach for

**CORRECTION (2026-08-22, caught by the operator).** An earlier version of this note said
Pixal3D "lost the mask lenses and spider emblem entirely" on the chibi figure. That was wrong,
and the cause was a rendering error, not the model: **Pixal3D's `to_glb` applies its own axis
conversion, so its meshes come out facing 180 degrees from TRELLIS and Hunyuan3D output.**
Rendered with the same camera, the "front" view was the back of the head — which of course has
no eyes and no chest emblem. Turned to face forward, Pixal3D has both, clearly.

The operator spotted it from the hands ("look at the thumbs"). Two process failures made it
possible: the render tool labels a view "front" by camera position rather than by anything
about the object, and a smooth blank head is exactly what a *plausible* bad result looks like,
so it never triggered a second look. **Anything that renders a mesh from a fixed camera needs
an orientation check before its output is compared.** Rotating the vertices with trimesh works;
setting `rotation_euler` in Blender and exporting silently did not, which is why the fix script
prints the vertex extent before and after and asserts it changed.

**Assessment.** Pixal3D produces the best-looking mesh of the three on both subjects: a solid
axe blade with a proper haft where TRELLIS and Hunyuan3D both tear it, crisper armour edges,
and a defined face. It keeps the spider emblem and mask lenses. Its costs are real but
narrow: ~3 minutes against 55 seconds, no texture on this build (nvdiffrast will not compile),
and it needs `--voxel-mm 0.20` at print prep because its raw output is a double-layered open
shell. Hunyuan3D-2mv remains the low-effort choice — fast, textured, watertight without
workarounds, best minimum wall (0.052mm goblin, 0.642mm spider-man). TRELLIS.2 needs no ROCm
build at all, since trellis.cpp runs on Vulkan.

**For a figure that will be printed and painted**, speed and texture are worth little, so the
ranking is on shape alone and Pixal3D leads.

**Caveat, stated because it weakens the comparison.** The three models received *different*
background mattes: BiRefNet for TRELLIS, Hunyuan's own encoder, and a u2net cutout for Pixal3D
(routing around the gated `briaai/RMBG-2.0`). Pixal3D also decimates and UV-remeshes inside
`to_glb`, which the other two do not. Some of its softer surface may come from that rather
than from the model.

## Related

- [[19-building-hip-extensions-on-strix-halo]]
- [[17-generating-3d-versus-scripting-it]]
