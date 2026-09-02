---
title: "Sega Saturn Game Guides"
type: index
category: games/saturn
tags: [saturn, sega-saturn, sega, guides, walkthrough]
draft: false
---

# Sega Saturn Game Guides

The Saturn lost. It was launched early and unannounced in North America, it was built around 2D
sprite hardware just as the industry turned to 3D polygons, it had two main processors that were
difficult to program, and Sega abandoned it for the Dreamcast in 1998.

What that left behind is a library of Japanese games with very small Western print runs, several of
which are now among the most expensive cartridges and discs in the hobby, and almost none of which
have ever been re-released.

| Game | Genre | Rating |
|------|-------|--------|
| [Panzer Dragoon Saga](topics/games/saturn/panzer-dragoon-saga) | RPG | 9.5/10 |
| [Guardian Heroes](topics/games/saturn/guardian-heroes) | Beat 'em up | 9/10 |
| [NiGHTS into Dreams](topics/games/saturn/nights-into-dreams) | Action | 9/10 |
| [Dragon Force](topics/games/saturn/dragon-force) | Strategy RPG | 9/10 |
| [Shining Force III](topics/games/saturn/shining-force-iii) | Strategy RPG | 9/10 |
| [Burning Rangers](topics/games/saturn/burning-rangers) | Action | 8/10 |
| [Sega Rally Championship](topics/games/saturn/sega-rally-championship) | Racing | 8.5/10 |

## Why these are hard to own

**Panzer Dragoon Saga** shipped roughly 20,000 English copies in 1998, after Sega had already
written the console off. A complete original sells for four figures. It has never been re-released
in any form, and Sega has said the source code was lost.

**Shining Force III** is three games in Japan telling one war from three armies' viewpoints, with
saves carrying between them. Only Scenario 1 was translated — the disc here — before Sega of America
stopped supporting the console. Fans finished the other two decades later.

**Burning Rangers** was the last thing Sonic Team made for the Saturn, in 1998, and has never
appeared anywhere since.

## Emulation notes that apply to all of them

The Saturn is genuinely hard to emulate — two SH-2 processors, a dedicated sound CPU and several
custom chips, all of which games used in idiosyncratic ways. This box uses the **beetle-saturn**
core, which is the accurate one rather than the fast one, and the UM790Pro has the headroom for it.

**BIOS:** the Saturn needs its boot ROM, which is installed and checksum-verified here. Batocera
reports two further files missing — the `.ic1` CD-block ROMs — and that is expected and deliberate:
beetle-saturn does not use them.

**Rewind is off for the Saturn** on this box. Two of these seven are turn-based strategy games where
rewind deletes the decision rather than smoothing a mistake, and the core is demanding enough that
rewind costs real performance. Save states are available.

**Panzer Dragoon Saga is four discs**, handled by an `.m3u` playlist so it appears as one entry and
swaps discs from the emulator's disc-control menu.

Back to [Batocera Top Games](topics/games/batocera-top-games).
