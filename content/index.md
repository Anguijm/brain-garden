---
title: B2 Electric Bugaloo
---

A public garden of notes I've read, verified, and chosen to keep. Each one was
promoted out of a private staging area, so it reflects settled understanding rather
than raw research. Newest is first.

## Latest

- **[Game guides for every game on the machine](topics/games/batocera-top-games)** — 163
  standalone walkthroughs covering all 169 titles installed on the UM790Pro, across 16 systems:
  [NES](topics/games/nes/), [SNES](topics/games/snes/), [N64](topics/games/n64/),
  [Game Boy](topics/games/gb/), [Game Boy Color](topics/games/gbc/),
  [Game Boy Advance](topics/games/gba/), [Mega Drive](topics/games/megadrive/),
  [Master System](topics/games/mastersystem/), [Game Gear](topics/games/gamegear/),
  [PC Engine](topics/games/pcengine/), [Saturn](topics/games/saturn/),
  [Dreamcast](topics/games/dreamcast/), [PlayStation](topics/games/psx/),
  [PS2](topics/games/ps2/), [PSP](topics/games/psp/) and [Neo Geo](topics/games/neogeo/).
  Each guide covers controls, the systems that actually matter, and what changes under emulation
  rather than on original hardware.

- **[Top 10 games on every system Batocera can run](topics/games/batocera-top-games)** —
  the wish list: 277 games across 27 systems (Master System through Wii U), ranked by critical
  consensus with honest caveats on PS3/RPCS3 and Xbox 360/Xenia compatibility. Now carries a
  section separating that list from what is actually installed and verified.

- **[The games Batocera ships with](topics/games/bundled-homebrew)** — the six freeware
  homebrew titles that come with a fresh install, and why they are there.

- **[Console jailbreaks: which are worth your time](topics/technology/console-jailbreak-landscape)** —
  ranked survey of every modern platform: 3DS (do it today), unpatched Switch (best current-gen
  option), PS Vita, PS4 below 9.00, PS3, Wii/WiiU, Xbox (no public jailbreak), PS5 (too early).
  With a comparison table and the pattern that explains why Nintendo hardware is always most
  exploitable.

- **[PS4 custom firmware: how the exploit chain works](topics/technology/ps4-cfw)** —
  the three-layer chain (WebKit → sandbox escape → kernel), generation-by-generation kernel
  exploits from firmware 1.76 through 7.02, why the PS4 fight is structurally different from
  the PS3 break, and where the 9.00/GoldHEN gap sits in the public record.

- **[How the PlayStation 3 got cracked permanently](topics/technology/playstation-hacks)** —
  Sony's cryptographic self-defeat: one constant in the wrong place, the root of trust burned
  into read-only hardware, and why there was never a patch. The geohot/fail0verflow story, the
  ECDSA flaw, the legal battle, and what it means about trust models generally.

- **[Shipyards and the maritime industrial base](topics/shipyards/)** — standing background
  for the weekly brief, kept separately because it outlives any one issue. Opens with
  [four threads from August 2026](topics/shipyards/2026-08-threads): the NAVSEA board whose
  job is cutting shipyard labour hours and which openly takes submissions, why the word
  "non-structural" is the whole story in the Navy's first organic cold spray centre, the
  additive qualification path where three of four gates are about the shop rather than the
  part, and why the \$76.6bn submarine award is an automation story rather than a hiring one.

- **[AI stock-picking services, investigated](topics/finance/ai-stock-picking/)** — a four-note,
  honestly-labeled teardown of the "AI stock pick" industry, heavy on peer-reviewed sources and
  built to separate wheat from chaff. [How the methods work](topics/finance/ai-stock-picking/)
  (multi-factor scores, "fair value" numbers, machine-learning models, sentiment, chart robots)
  and the five ways each misleads; [the landscape](topics/finance/ai-stock-picking/the-landscape)
  of what InvestingPro, Danelfin, Zacks, Trade Ideas, Tickeron and the rest actually sell;
  [whether any of it beats the market](topics/finance/ai-stock-picking/does-it-actually-work) (the
  SPIVA base rates, skill-versus-luck, the Gu-Kelly-Xiu machine-learning evidence and what erodes
  it); and [how to emulate it without fooling yourself](topics/finance/ai-stock-picking/build-it-yourself).
- **[Mini forge](projects/mini-forge/)** — a local pipeline that turns a text prompt into a
  printable tabletop miniature with no cloud service in the loop: an AMD Strix Halo box with
  64 GB of unified memory, TRELLIS.2 running on Vulkan, and headless Blender for the print
  preparation. The machines, the choices, the traps already paid for, and an honest list of
  what has not been proven yet.
- **[The Waterfront Brief](projects/waterfront-brief/)** — a weekly four-page intelligence
  brief for naval shipyard management: waterfront ops, maintenance technologies, readiness,
  and business practices, compiled from NAVSEA, USNI News, Naval News, Drydock Magazine,
  The Maritime Executive, and GAO, with every claim labeled and sourced. Print edition is
  a two-column, exactly-four-page PDF.
- **[Building your own basketball spread model](topics/games/basketball-ats-model/)** — how
  to develop and train a model to predict NBA and NCAA games against the spread: the data
  stack, the features that matter, walk-forward validation, calibration — and the honest
  numbers on why the spread market usually wins (49.86% dog cover rate over 10,325 games;
  52.38% break-even; edges that flip between eras), plus a companion on how prediction
  research itself works: the discovery loop, the tabular state of the art, and the
  parlay math of modern sportsbooks. Illustrated.
- **[Leadership and management: a working knowledge base](topics/leadership/)** — plain-English,
  honestly-labeled notes on the most-cited ideas in management and leadership, built from
  nearly fifty HBR classics (Goleman, Herzberg, Mintzberg, Collins, Hill, Hackman, Buckingham,
  Edmondson, Amabile, Pentland, Ericsson, Cross, Zak, and more), each with a "how much to trust
  this" section, plus a living **[Ten Rules](topics/leadership/ten-rules)** list that updates as
  the library grows. Illustrated.
- **[Supplements for a midlife body: help, not hurt](topics/health/supplements-for-midlife/)**
  — an honest, evidence-based guide for a 48-year-old overweight man who's training: the short
  stack that actually works (creatine, protein, vitamin D, psyllium), what's situational, and
  the real list of things to avoid, with safety and bloodwork first. Illustrated.
- **[Muscle, aging, and "anabolic resistance"](topics/health/muscle-protein-synthesis-aging/)**
  — the real science behind a muscle-supplement pitch: how muscle stops responding to protein
  as you age, what leucine and HMB actually do, and an honest verdict on Apex Muscle Defense
  (real science, oversold product). Illustrated.
- **[Mobile game engines you can drive from the CLI](topics/software/mobile-game-engines/)** —
  a survey of free, hobbyist-friendly engines for building Android and iOS games from the
  command line (Godot, Defold, Solar2D, Flame, libGDX, MonoGame, raylib, Ebitengine,
  macroquad), with the real build commands and honest caveats. Illustrated.
- **[Godot, a plain-English course](topics/software/godot/)** — learn the free, open-source
  game engine from zero: nodes and scenes, the editor, GDScript, signals and the game loop,
  building a small game, and exporting it. Illustrated.
- **[Plastic and tissue: 3D-printed flying model aircraft](topics/making/3d-printed-tissue-aircraft/)**
  — replacing balsa with X1C-printed LW-PLA frames and keeping the tissue: how printed
  airframes are built, where it flies (and where balsa still wins), and the honest
  catch in covering plastic.
- **[AI agent engineering — a working reference](topics/ai-engineering/)** — a full
  wing on building with LLMs: workflows vs agents, tools and MCP, memory, context
  engineering, RAG, evaluation, multi-agent systems, safety, and a deep dive on
  MRAgent (reconstructive memory). Illustrated.
- **[Weighty board game pieces with cold-casting](topics/making/cold-cast-game-pieces/)**
  — the heavier mold-and-cast route off a Bambu X1C: metal-resin with steel/tungsten
  fill for real heft (companion to the electroplating piece).
- **[Metal-look board game pieces at scale](topics/making/plated-game-components/)** —
  3D printing plus electroplating to give coins, tokens, and minis real metal heft,
  how to batch it, plus the safety and the IP line.
- **[Homebrew on portable game devices](topics/games/portable-homebrew/)** — a
  high-level, plain-English tour of the homebrew scene: what it is (and isn't), why
  people do it, the open-vs-locked landscape, the PSP era, and today's handhelds.
- **[Navisworks Freedom 2026 — a complete course](topics/software/navisworks-freedom-2026/)**
  — using Autodesk's free 3D model viewer: opening published models, navigating,
  reading properties, viewpoints and clash results, sectioning, measuring, and 4D
  playback.
- **[EP-133 K.O. II — a complete course](topics/music/ep-133-ko2/)** — a
  beginner-to-advanced course for Teenage Engineering's sampler: every button, then
  sampling, sequencing, effects, MIDI, resampling, and genre recipes.
- **[Defense BD Playbook](topics/business-development/defense-bd-playbook/)** — a
  working playbook for winning U.S. defense business: the capture lifecycle,
  qualification, teaming, proposals, pricing, and using Gen AI without fooling
  yourself.
- **[Rice cooker recipes](topics/cooking/rice-cooker-recipes)** — the few techniques
  that matter, plus one-pot meals that go well beyond plain rice.
- **[White's (dumpy) tree frogs](topics/pets/whites-tree-frog-bioactive-japan)** —
  care basics plus a bioactive terrarium build for Japan.
- **[Puppy training](topics/pets/puppy-training)** — a practical starter guide:
  reward-based basics, socialization, house and crate training.
- **[Sudoku: basics to advanced](topics/games/sudoku/)** — a 13-lesson, plain-English
  solving curriculum, from the one rule up through wings, fish, and chains.

## Explore by thread

Filed by topic above, but the more interesting structure runs sideways: the same ideas
recur across unrelated notes. The **[Connections](connections/)** pages pull single
threads through several domains:

- **[Using AI well, without fooling yourself](connections/using-ai-well)** — the same
  discipline in AI engineering, defense BD, and (surprisingly) a frog terrarium.
- **[Making with the Bambu X1C](connections/making-with-the-x1c)** — one printer, heavy
  game pieces to ultralight airframes.
- **[When the artifact looks right and isn't](connections/looks-right-is-wrong)** — the same
  failure in five domains: output that is well-formed, plausible and wrong, so every cheap check
  passes. A scraper that named Mega Man 2 "Totally Rad", a vendor backtest you cannot falsify, a
  search summary with the wrong date.
- **[Finite resources, ruthless economy](connections/finite-resources)** — tokens,
  megabytes, grams, and people-hours, one shared move.

## How this place works

- **[How the vault actually runs](areas/how-the-vault-runs)** — the sequence map. What happens when a
  newsletter gets made, what happens when a topic gets researched, which parts are on a timer, and
  exactly where the five model calls sit. Short version: almost none of it is an autonomous agent.

## Finding your way around

Use the **explorer** on the left (the menu button on a phone) to browse everything
by topic, and the **search** box to jump straight to anything. The **graph** view and
the **theme tags** (`verification`, `finite-resources`, `maker-toolchain`, and friends)
are the best way to stumble onto a connection. Every external source cited anywhere in
the garden is listed in the **[Source register](sources)**, article by article, in order
of appearance. New notes appear at the top of the list above as they're promoted.
