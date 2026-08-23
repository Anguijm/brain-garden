---
type: note
series: mini-forge
title: "Mini forge"
status: curated
tags: [3d-printing, miniatures, amd, strix-halo, vulkan, provisioning, openscad]
created: 2026-08-19
---

# Mini forge

A local pipeline that turns a text prompt into a printable tabletop miniature, running
entirely on hardware in the house with no cloud service in the loop. This page is the
working documentation: what the machines are, what the pipeline does, why each piece was
chosen, and what is still unproven.

The reasoning behind the approach is in
[Generating 3D, and why you cannot just script it](topics/ai-engineering/17-generating-3d-versus-scripting-it).
The lessons from building it are in
[The plan that reads right and the plan that runs](topics/ai-engineering/18-the-plan-that-runs).

## The machines

| | RTX 3070 laptop | MINISFORUM MS-S1 MAX |
|---|---|---|
| Role | vault machine, first tests | the mini forge |
| CPU | i7-10750H, 6 cores | Ryzen AI Max+ 395, 16 Zen 5 cores |
| GPU | RTX 3070 Laptop, **8 GB VRAM** | Radeon 8060S iGPU, RDNA 3.5, gfx1151 |
| Memory | 15.8 GB, 15.4 GB zram swap | **128 GB** LPDDR5X, unified |
| OS | Pop!_OS | Ubuntu 24.04.4 LTS |

A third machine, a MINISFORUM UM790 Pro, was considered and left out. Its Radeon 780M is a
12-compute-unit part against the 8060S's 40, so it adds a machine to maintain and nothing
to the pipeline.

**Unified memory is the whole reason the second machine matters.** On a discrete card, VRAM
is a hard wall, and 8 GB rules out the better models. On Strix Halo the GPU addresses system
RAM, so the working set that was impossible on the laptop is unremarkable on the mini PC.

## The pipeline

```
words -> picture -> mesh -> printable STL -> printer
```

Each stage, and what it is rather than what it is called:

| Piece | What goes in | What comes out | What it is for |
|---|---|---|---|
| **Image generator** (not chosen yet) | a sentence | one PNG picture | gives the mesh maker something to look at |
| **trellis.cpp** | one PNG | a GLB mesh | guesses the whole 3D shape, including the back you cannot see |
| **the weights** (10 `.gguf` files, 16.5 GB) | nothing, they just sit there | nothing | the learned experience trellis.cpp reasons with; without them it is an engine with no fuel |
| **Vulkan** | nothing | nothing | how the program talks to the graphics chip so the maths runs on the GPU instead of the CPU |
| **Blender + `prepare_mini.py`** | a GLB mesh | an STL | scales it to 32 mm, adds a 25 mm base, fills holes so a slicer can tell inside from outside |
| **slicer** (Bambu Studio, on the printer's machine) | an STL | printer instructions | the last step, and the only one not on this box |

Two words that get used loosely and mean specific things here. A **mesh** is not a program, it is
the object: a skin of triangles describing a surface, like chicken wire bent into a goblin. A **GLB**
is a mesh file with its colours attached; an **STL** is a mesh file with no colours, which is all a
printer needs.

**OpenSCAD is not part of this chain.** It is the other half of the machine's job: you write code
describing exact shapes and it builds them. Good at brackets and boxes, bad at goblins. The two
approaches fail at opposite things, which is the subject of
[chapter 17](topics/ai-engineering/17-generating-3d-versus-scripting-it).

**The mesh step is `trellis.cpp`**, a C++/GGML reimplementation of Microsoft's TRELLIS.2-4B
with Vulkan, CUDA, ROCm and CPU backends and no Python at runtime. Chosen over TripoSR
(much weaker geometry) and over a PyTorch TRELLIS install, which needs a CUDA toolkit this
machine has no use for. Its README reports Vulkan as the fastest backend on Strix Halo,
ahead of ROCm by 10 to 40 per cent.

**Two GPU stacks on purpose.** Vulkan for meshing, because RADV needs no setup and is
faster here. ROCm for image generation, because PyTorch and ComfyUI have no Vulkan path.
This is not indecision and should not be "simplified" into one.

**Weights: the ten full-precision files, about 16.5 GB.** The HuggingFace repository
`ilintar/trellis2-gguf` is 33 GB because it also carries `q4/` and `q8/` variants. With
64 GB of unified memory the quantized copies buy nothing.

**Print preparation order matters and is usually given wrong.** Import, scale, boolean-union
the base, then voxel remesh **last**. Remeshing before the boolean lets the union
reintroduce non-manifold geometry at the seam, which is exactly what the remesh was for.

## The scripts

All in `_scripts/` in the brain vault.

| Script | What it does |
|---|---|
| `prep-ms-s1-media.sh` | Downloads and **verifies** the Ubuntu ISO, the BIOS package, the UEFI shell, trellis.cpp, a test image, and the 16.5 GB of weights into `~/ms-s1-setup`. Non-destructive. |
| `make-bios-usb.sh` | Builds the BIOS-flash stick. Guarded: removable and USB only, never a system disk, retype to confirm. |
| `export-claude-env.sh` | Splits Claude Code config from secrets so the environment can travel. Plain by default, `--encrypt` available. |
| `make-ms-s1-payload.sh` | Adds a payload partition to the installer stick: repo bundles, weights, scripts, config. |
| `ms-s1-firstboot.sh` | On the new machine: toolchain, unified-memory tuning, builds trellis.cpp, and generates a first mini end to end. |
| `restore-claude-env.sh` | Rebuilds the Claude Code environment on the far side. |
| `prepare_mini.py` | Headless Blender: mesh in, printable STL on a 25 mm base out. |

## Decisions worth not relitigating

**Ubuntu 24.04.4 LTS, Windows wiped.** Originally 26.04, reversed on 2026-08-20 because
**26.04 does not boot on this machine**: it reaches its boot menu and then black-screens
when the kernel takes the display, on both HDMI and USB-C, surviving safe graphics,
`nomodeset` and `modprobe.blacklist=amdgpu`. The machine stays alive throughout; only the
display dies. There is a documented regression where Strix Halo black-screens on kernel
6.19.0 and works on 6.18.9, and 26.04 ships 7.0, which is 6.20 renumbered. 24.04 is what
both published Strix Halo guides actually run.

**No autoinstall.** Ubuntu Desktop 23.04 and later support cloud-init autoinstall via a
`CIDATA` volume, so pre-seeding the user and WiFi is possible. For one machine it costs more
to debug than the clicking it saves, and fails worse. A payload partition and a first-boot
script deliver the same outcome.

**WiFi, no ethernet.** The WiFi is a MediaTek MT7925, supported out of the box. The 10 GbE
ports are Realtek RTL8127, whose driver only reached upstream in kernel 6.16+, which
24.04's kernel predates, so those ports may not work until a newer kernel is installed. An
earlier draft of the plan had this backwards and recommended ethernet as the safer path.

**Do NOT carry the weights on the installer stick** (reversed 2026-08-20). Putting a
payload partition on a boot device was over-engineering, and it cost a GPT that believed
the disk ended where the ISO did, a deleted ISO padding partition, a retry path that
punished retries, and two evenings. Write the plain ISO. Move the data afterwards over the
network.

**The kernel has a floor and a ceiling.** 6.16.9 or newer for full memory access and for
the 10 GbE driver; but 6.19 and 7.0 black-screen this machine. That window is narrow and
it is the single most important fact on this page.

**Keep the BIOS graphics allocation small.** AMD's own guidance is to reserve little in
firmware (they suggest 0.5 GB) and raise the shared TTM/GTT limit on the Linux side, because
unified memory has no dedicated-is-faster advantage. Do not leave the setting on "Auto";
that has been reported to break unified-memory detection.

## Traps already paid for

- The TTM module is `ttm` with the in-kernel amdgpu and `amdttm` under AMD's DKMS build, so
  the documented `pages_limit` path is wrong half the time. Detect which is loaded.
- `pages_limit` is in **pages**, not GB. AMD ships `amd-ttm` to convert.
- MINISFORUM ships BIOS updates with a Windows-only tool, but the same archive contains
  `AfuEfix64.efi` and `EfiFlash.nsh`, which run from the EFI shell with no OS at all.
- **Read the installed BIOS version before building a flash stick.** The machine arrived on
  1.06, which is the version the stick would have written.

## Status, 2026-08-23

**The pipeline is complete and has produced print-ready files.** Picture in, sliced 3MF out,
entirely on the MS-S1.

**FACT — three generators run on the Radeon 8060S (gfx1151):**

| model | route | speed | notes |
|---|---|---|---|
| TRELLIS.2 (`trellis.cpp`) | **Vulkan**, no ROCm needed | 116s @ res 512 | simplest to keep working |
| Hunyuan3D-2mv | ROCm + ComfyUI | 55s | textured; **takes four view inputs** |
| Pixal3D | ROCm, native extensions | ~3 min | best surface detail; untextured here |

**FACT — multi-view conditioning works and measurably helps.** `Hunyuan3Dv2ConditioningMultiView`
accepts front, left, back and right. Same model and seed, one view versus four: the goblin's axe
goes from a torn flap to a clean blade, and a dwarf figure's hammers and hat brim gain real mass.
The improvement is largest on parts the single front view can only see edge-on.

**FACT — the slicer accepts the output.** Bambu Studio's CLI, driven with a real
`Bambu Lab X1 Carbon 0.4 nozzle` + `0.08mm Extra Fine` + `Bambu PLA Basic` profile, slices every
mesh with **no** warning about manifoldness, self-intersection, degenerate triangles or thin walls.
Nine figures at 48 mm print in 17 to 28 minutes each.

**Assessment — print bigger, it is the cheapest lever there is.** Same mesh, same 0.10 mm prep
voxel, three sizes: material under 0.30 mm falls from 1.92% at 32 mm, to 0.53% at 48 mm, to 0.05%
at 75 mm. Nothing about the shape changed; every feature simply got thicker. A finer prep voxel is
a *different* lever and does not help printability — it captures more of the generated detail, and
captures thin things accurately thin.

**The print-prep bug is fixed.** `prepare_mini.py` now ends in hard self-checks (height within one
voxel, sits on z=0, footprint, welded into one solid, watertight) and **exits 2** if any fail. The
missing base that prompted this is gone. Two habits came out of it and are worth keeping: measure
the output rather than reading the log, and render a preview and look at it, because a number and a
picture catch different faults.

**Known defects, characterised rather than guessed:**

- **Pixal3D emits a double-layered open shell** — 503,463 open edges across 43,089 components on the
  raw GLB, and two surfaces about 0.002 mm apart over the whole figure. A coarse voxel at print prep
  (0.20 to 0.30 mm, subject-dependent) fuses them and it slices normally. Fine voxels preserve the
  defect and Bambu rejects the object at load with `1 models, 0 objects`.
- **Pixal3D output faces 180 degrees from the others**, because its `to_glb` applies its own axis
  conversion. Rotating the vertices with trimesh works; setting `rotation_euler` in Blender and
  exporting silently does not.
- **nvdiffrast cannot be built here.** Its CUDA rasteriser is NVIDIA PTX inline assembly. It is used
  only for texture baking, so Pixal3D runs untextured — which costs nothing for a figure that will
  be painted.
- **CuMesh needed a source patch.** `hipMemcpy2D` rejects a pointer PyTorch's caching allocator hands
  out mid-pipeline, even though it is valid device memory. Since destination pitch equals row width,
  the 2D copy is a linear copy, and `hipMemcpy` accepts what `hipMemcpy2D` refused.

**Assessment — the input matters more than the model.** The best result so far came from a
purpose-made six-panel orbit sheet: consistent lighting, consistent scale, clean background, all
four sides. Everything improvised from photographs has been worse. For a real subject, a slow walk
all the way around with the camera level beats any choice of generator.

**Still unverified.** Nothing has actually been printed. Every claim here is about meshes and
slicer output, not about plastic.

## See also

- **[Generating 3D, and why you cannot just script it](topics/ai-engineering/17-generating-3d-versus-scripting-it)** — why a generated mesh and a scripted one fail at opposite things.
- **[The plan that reads right and the plan that runs](topics/ai-engineering/18-the-plan-that-runs)** — the six failures that building this turned up.
- **[Building HIP extensions against AMD's self-contained ROCm wheels](topics/ai-engineering/19-building-hip-extensions-on-strix-halo)** — how the ROCm half of this was made to compile, and the five packaging gaps that stop it.
- **[Three image-to-3D models, judged by the slicer instead of a ruler](topics/ai-engineering/20-three-image-to-3d-models-against-a-real-slicer)** — the comparison that decides which generator to reach for, and why the thickness metric was the wrong gate.

## Sources

- pwilkin, *trellis.cpp* — https://github.com/pwilkin/trellis.cpp
- ilintar, *trellis2-gguf weights* — https://huggingface.co/ilintar/trellis2-gguf
- AMD, *Strix Halo system optimization* — https://rocm.docs.amd.com/en/docs-7.2.0/how-to/system-optimization/strixhalo.html
- capetron, *MS-S1 Max BIOS update from Linux* — https://github.com/capetron/minisforum-ms-s1-max-bios
