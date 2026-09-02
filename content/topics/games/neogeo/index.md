---
title: "Neo Geo Game Guides"
type: index
category: games/neogeo
tags: [neogeo, snk, arcade, guides, walkthrough]
draft: false
---

# Neo Geo Game Guides

SNK's Neo Geo was arcade hardware sold, unchanged, as a home console in 1990 — the AES, at around
\$650 with cartridges near \$200 each. Almost nobody bought one. The arcade version, the MVS, was
everywhere.

Because the two were the same machine, the home library is the arcade library with nothing cut, and
the hardware stayed in production until 2004. Everything below is the arcade game.

| Game | Genre | Rating |
|------|-------|--------|
| [Metal Slug](topics/games/neogeo/metal-slug) | Run and gun | 9.5/10 |
| [Metal Slug 3](topics/games/neogeo/metal-slug-3) | Run and gun | 9.5/10 |
| [Garou: Mark of the Wolves](topics/games/neogeo/garou-mark-of-the-wolves) | Fighting | 9.5/10 |
| [The King of Fighters '98](topics/games/neogeo/king-of-fighters-98) | Fighting | 9/10 |
| [Samurai Shodown II](topics/games/neogeo/samurai-shodown-2) | Fighting | 9/10 |
| [The Last Blade 2](topics/games/neogeo/last-blade-2) | Fighting | 8.5/10 |
| [Blazing Star](topics/games/neogeo/blazing-star) | Shoot 'em up | 8.5/10 |
| [Pulstar](topics/games/neogeo/pulstar) | Shoot 'em up | 8.5/10 |
| [Neo Turf Masters](topics/games/neogeo/neo-turf-masters) | Sports | 8/10 |
| [Shock Troopers](topics/games/neogeo/shock-troopers) | Run and gun | 8/10 |

## These are arcade romsets, not cartridge dumps

Every other system on this box uses No-Intro or Redump cartridge and disc dumps. Neo Geo does not:
it uses **FinalBurn Neo arcade romsets**, which are zip files of individual chip dumps named by a
short set code — `mslug.zip`, `garou.zip` — rather than by title. The emulator needs those exact
names, which is why the ROM files look nothing like the games.

They also need **`neogeo.zip` as a system BIOS**, in `/userdata/bios`. Without it nothing loads.

**Romsets are version-locked to the emulator core.** A set built for a different FinalBurn Neo
release fails a checksum at load. There is no way to confirm compatibility from metadata, so all ten
here were launch-tested on this machine before being trusted.

One quirk worth knowing so it is not chased later: `batocera-systems` reports our `neogeo.zip` as
**UNTESTED** because its MD5 differs from the one Batocera lists. The contents are correct — it
carries `sfix.sfix` and `000-lo.lo` at exactly the CRCs the core asks for — and every game loads. It
is a packaging difference, not a fault.

## Names that will catch you out

Arcade set codes are terse and the near-misses are dangerous:

- **`turfmast`** is Neo Turf Masters. **`tturf`** in the same collection is *Tough Turf*, an
  unrelated Sega beat 'em up.
- **`garou`** is Mark of the Wolves. Beside it sit `garoubl` (bootleg), `garoup` (prototype) and
  `garouh` (alternate revision).
- **`kof98`** and **`samsho2`** also exist in that collection as **Mega Drive, SNES and Neo Geo
  Pocket** games. Same filenames, different games entirely.

## Continues change these games

All of these were built to consume coins, and several — Metal Slug 3's final mission especially —
are balanced around a player who keeps paying. Emulated, continues are free, which turns a
deliberately unfair design into something finishable. That is a legitimate way to see them.

Back to [Batocera Top Games](topics/games/batocera-top-games).
