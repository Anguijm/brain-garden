---
title: "TurboGrafx-16 Game Guides"
type: index
category: games/pcengine
tags: [pcengine, turbografx-16, nec, guides, walkthrough]
draft: false
---

# TurboGrafx-16 Game Guides

Standalone walkthroughs for the TurboGrafx-16 games on the UM790Pro. NEC's console lost the
console war in North America and won it comfortably in Japan as the PC Engine, which is why its
library is both excellent and unfamiliar. Everything here runs on Batocera's PC Engine core.

| Game | Genre | Rating |
|------|-------|--------|
| [Blazing Lazers](topics/games/pcengine/blazing-lazers) | Shoot-em-up | 9.5/10 |
| [Bonk's Adventure](topics/games/pcengine/bonks-adventure) | Platformer | 8.5/10 |
| [Military Madness](topics/games/pcengine/military-madness) | Turn-based strategy | 9/10 |
| [Dungeon Explorer](topics/games/pcengine/dungeon-explorer) | Action-RPG | 8.5/10 |
| [Neutopia](topics/games/pcengine/neutopia) | Action-adventure | 8.5/10 |
| [Magical Chase](topics/games/pcengine/magical-chase) | Shoot-em-up | 9/10 |

## Two things that apply across the system

**Integer scaling earns its keep here.** The TurboGrafx outputs 256x239, which divides evenly
into a 4K panel, so every source pixel becomes a uniform block rather than being smeared across
a fractional multiple. The sprite art on this system — Magical Chase and Bonk especially — is
the main reason to care.

**Rewind is enabled for this system on the box.** It suits the two shooters best: in Blazing
Lazers a death strips your entire accumulated loadout, so an un-rewound mistake late in a run
effectively ends it. The exception is Military Madness, where rewinding lets you undo an attack
after seeing the result and quietly removes the permanence that gives the game its weight.

## Four still missing

The top-10 list for this system names ten games; six are on the box. **Castlevania: Rondo of
Blood, Ys Book I & II, Gate of Thunder and Lords of Thunder** are all PC Engine **CD** titles,
and the source we used carries HuCard releases only. They need a CD-ROM² source and Batocera's
`pcenginecd` folder rather than `pcengine`.

Back to [Batocera Top Games](topics/games/batocera-top-games).
