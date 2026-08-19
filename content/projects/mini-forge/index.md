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
| Memory | 15.8 GB, 15.4 GB zram swap | 64 GB LPDDR5X, **unified** |
| OS | Pop!_OS | Ubuntu 26.04 LTS |

A third machine, a MINISFORUM UM790 Pro, was considered and left out. Its Radeon 780M is a
12-compute-unit part against the 8060S's 40, so it adds a machine to maintain and nothing
to the pipeline.

**Unified memory is the whole reason the second machine matters.** On a discrete card, VRAM
is a hard wall, and 8 GB rules out the better models. On Strix Halo the GPU addresses system
RAM, so the working set that was impossible on the laptop is unremarkable on the mini PC.

## The pipeline

```
text prompt
   -> image model (ComfyUI on ROCm)            a character on a plain background
   -> trellis.cpp on Vulkan                    textured GLB mesh
   -> Blender, headless (prepare_mini.py)      printable STL
```

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

**Ubuntu 26.04 LTS, Windows wiped.** It carries ROCm in the archive and its tested
architecture list names gfx1151. Fedora and Arch get newer ROCm sooner; neither is an LTS.

**No autoinstall.** Ubuntu Desktop 23.04 and later support cloud-init autoinstall via a
`CIDATA` volume, so pre-seeding the user and WiFi is possible. For one machine it costs more
to debug than the clicking it saves, and fails worse. A payload partition and a first-boot
script deliver the same outcome.

**WiFi, no ethernet.** The WiFi is a MediaTek MT7925, supported out of the box. The 10 GbE
ports are Realtek RTL8127, whose driver only reached upstream in kernel 6.16+. An earlier
draft of the plan had this backwards.

**Carry the weights on the stick.** 16.5 GB over WiFi is a much worse afternoon than
copying it here.

**Kernel 6.18.4 is a hard floor** for the AMD KFD fixes Strix Halo needs. Ubuntu 26.04
clears it comfortably.

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

## Status, 2026-08-19

Assessment: media preparation is done and verified. The machine has not been installed yet.

- Done: ISO downloaded and checksum-verified, BIOS package extracted, 16.5 GB of weights
  complete, Claude Code environment exported, all scripts written and their destructive
  paths guarded.
- Done: BIOS flash determined **unnecessary**; the machine shipped on 1.06.
- Next: write the Ubuntu installer to the stick, add the payload partition, install, run
  first boot.

**Unverified, and the honest list.** None of the trellis.cpp path has run on Strix Halo
hardware from here. The build, the Vulkan backend on gfx1151, the published timing of about
345 seconds for a 1024-resolution mesh, and whether the payload partition can be added
cleanly after a hybrid ISO layout are all still claims rather than observations. The first
run on the machine tests all of them at once.

## See also

- **[Generating 3D, and why you cannot just script it](topics/ai-engineering/17-generating-3d-versus-scripting-it)** — why a generated mesh and a scripted one fail at opposite things.
- **[The plan that reads right and the plan that runs](topics/ai-engineering/18-the-plan-that-runs)** — the six failures that building this turned up.

## Sources

- pwilkin, *trellis.cpp* — https://github.com/pwilkin/trellis.cpp
- ilintar, *trellis2-gguf weights* — https://huggingface.co/ilintar/trellis2-gguf
- AMD, *Strix Halo system optimization* — https://rocm.docs.amd.com/en/docs-7.2.0/how-to/system-optimization/strixhalo.html
- capetron, *MS-S1 Max BIOS update from Linux* — https://github.com/capetron/minisforum-ms-s1-max-bios
