---
type: lesson
series: mobile-game-engines
chapter: 1
title: Full engines
status: curated
tags: [software, game-development, mobile, cli, godot, defold, solar2d]
created: 2026-07-01
---

# Full engines you can drive from the command line

These three come with a visual editor, so they are not "command line only," but each one can
also build and export from a terminal, which is what makes them fit here. They do the most
for you of any tools in this survey.

## Godot

FACT: Godot is a free engine for 2D and 3D games, scripted mainly in its own GDScript
language (C# is also supported), under the permissive MIT license. The current version is
4.7, released in June 2026, and it is very actively developed. (Godot docs; godotengine.org.)

FACT: you can export a mobile build from the command line with no editor window. For Android,
the pattern is a single headless command:

```bash
godot --headless --export-release "Android" game.apk
```

The name in quotes must match an export preset you set up, and export templates for your
engine version must be installed first, or the command fails. iOS uses the same command with
the iOS preset, but it produces an Xcode project that you finish on a Mac. (Godot docs,
*Command line tutorial* and *Exporting projects*.)

Assessment: Godot is the default recommendation for most people. It is the most popular of
the free engines, does both 2D and 3D, has by far the largest community and tutorial supply,
and its mobile export is mature. There is a full [Godot course](../godot) in this garden,
including a chapter on exporting. The only catch, shared by everything here, is that the CLI
build assumes you have already set up the Android SDK and a signing key.

## Defold

FACT: Defold is a free engine backed by the non-profit Defold Foundation (it began at the
game company King). It is scripted in Lua, focused on 2D with some 3D ability, and it exports
to Android and iOS. Its current version is 1.13, released in June 2026, and it is very
active. (defold.com; Defold GitHub.)

FACT: Defold ships a command-line build tool called `bob` (a single `bob.jar`), which builds
and bundles a mobile app headlessly. For Android:

```bash
java -jar bob.jar --archive --platform armv7-android resolve distclean build bundle
```

iOS uses `--platform arm64-ios` in the same command. It needs a JDK installed. (Defold docs,
*Bob*.)

FACT: the license is a big selling point. Defold uses its own developer-friendly license
(derived from Apache 2.0), and the Foundation states the engine "will always remain
completely free," with no royalties, no fees, and no subscriptions. One limit worth knowing:
you can freely sell the games you make, but you may not sell the engine or editor itself.
(defold.com/license.)

Assessment: Defold is an excellent free, no-royalty choice, with a clean single-jar CLI that
is genuinely pleasant for automated builds, and a batteries-included editor if you want one.
Its community is smaller than Godot's, so you will find fewer tutorials, but the engine
itself is solid and well maintained.

## Solar2D

FACT: Solar2D is the free, open-source successor to the old Corona SDK. It is a mobile-first
2D engine, scripted in Lua, under the MIT license, and it targets iOS and Android (plus TV
platforms, desktop, and web). Its releases are current: version 2026.3730 came out in May
2026, with a steady cadence through 2025 and 2026. (solar2d.com; Solar2D GitHub.)

FACT: the scriptable command-line build tool is `CoronaBuilder`, which runs a small "recipe"
file that names the platform, app details, and signing key:

```bash
CoronaBuilder build --lua recipe.lua
```

The recipe sets `platform = "android"` or `"ios"`; iOS builds still require a Mac with Xcode.
(Solar2D build automation docs.) The older path of opening the Simulator and choosing a build
menu still exists, but `CoronaBuilder` is the one you script.

Assessment: Solar2D is a pleasant, genuinely mobile-first way to make 2D phone games in Lua,
and it is still actively maintained despite its Corona roots. The honest caution is that it is
a small, community-run project now, so it carries more single-maintainer risk than Godot or
Defold. If you like Lua and want a mobile-first tool, it is worth a look; just go in aware
that the ecosystem is older and smaller than it once was.

## Sources

- Godot, command-line export — https://docs.godotengine.org/en/stable/tutorials/editor/command_line_tutorial.html
- Godot, exporting projects — https://docs.godotengine.org/en/stable/tutorials/export/exporting_projects.html
- Defold, Bob command-line tool — https://defold.com/manuals/bob/
- Defold, license — https://defold.com/license/
- Solar2D — https://solar2d.com/
- Solar2D releases — https://github.com/coronalabs/corona/releases
