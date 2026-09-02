---
title: "Game Boy Advance Game Guides"
type: index
category: games/gba
tags: [gba, gameboy-advance, nintendo, guides, walkthrough]
draft: false
---

# Game Boy Advance Game Guides

The Game Boy Advance is roughly a portable Super Nintendo with a worse screen and better sound
hardware than it was allowed to use. It got a decade of Nintendo's best 2D work, plus ports of
things that had no business running on a handheld.

| Game | Genre | Rating |
|------|-------|--------|
| [Metroid Fusion](topics/games/gba/metroid-fusion) | Metroidvania | 9.5/10 |
| [The Legend of Zelda: The Minish Cap](topics/games/gba/minish-cap) | Action-adventure | 9/10 |
| [Castlevania: Aria of Sorrow](topics/games/gba/castlevania-aria-of-sorrow) | Metroidvania | 9.5/10 |
| [Advance Wars](topics/games/gba/advance-wars) | Turn-based strategy | 9.5/10 |
| [Fire Emblem: The Sacred Stones](topics/games/gba/fire-emblem-sacred-stones) | Strategy RPG | 9/10 |
| [Final Fantasy VI Advance](topics/games/gba/final-fantasy-vi-advance) | RPG | 9.5/10 |
| [Golden Sun](topics/games/gba/golden-sun) | RPG | 8.5/10 |
| [Pokémon Emerald](topics/games/gba/pokemon-emerald) | RPG | 9/10 |
| [Tactics Ogre: The Knight of Lodis](topics/games/gba/tactics-ogre-knight-of-lodis) | Strategy RPG | 8.5/10 |

## The screen problem, and the fix

The original Game Boy Advance had **no backlight**. Every game on this list was coloured by artists
compensating for a dim, unlit screen — brightening palettes so they would read at all under a
desk lamp.

Put those same games on a bright 4K panel with no correction and they look **bleached and flat**,
which is the exact opposite of the problem the art was solving. Batocera's GBA colour-correction
shader reverses the compensation and is on for this system on this box. It matters most on the
dark games — Metroid Fusion, Aria of Sorrow — and on Final Fantasy VI Advance, which otherwise
looks visibly paler than the SNES original sitting on the same machine.

## Rewind is off for the strategy games

**Advance Wars**, **Fire Emblem: The Sacred Stones** and **Tactics Ogre** have rewind disabled here
deliberately. In a turn-based game rewind does not smooth over a mistimed jump, it deletes the
decision — and in Fire Emblem specifically it removes permadeath, which is the mechanic the entire
design rests on. Everything else on the system has rewind on.

## One game you have twice

**Final Fantasy VI Advance** is the same game as the SNES cartridge on this box labelled
*Final Fantasy III*. Square renumbered the Western releases. The Advance version has the better
translation, the bug fixes and four extra dungeons; the SNES version has better sound. Both are
here.

## One game that is half a story

**Golden Sun** ends on a cliffhanger and finishes in *The Lost Age*, which imports your save. The
password transfer works normally under emulation.

Back to [Batocera Top Games](topics/games/batocera-top-games).
