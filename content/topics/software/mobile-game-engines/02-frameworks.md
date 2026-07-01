---
type: lesson
series: mobile-game-engines
chapter: 2
title: Frameworks
status: curated
tags: [software, game-development, mobile, cli, flutter, flame, libgdx, monogame, haxeflixel]
created: 2026-07-01
---

# Frameworks: write code in a friendly language

These four give you a game library and a comfortable programming language, and you build the
game by writing code rather than clicking around an editor. All four are free, mature, and
built from the command line.

## Flutter + Flame

FACT: Flame is a 2D game engine written in Dart that runs on top of Flutter, Google's
mobile-first app toolkit. Flame handles the game loop, sprites, collisions, and particles;
Flutter handles the app and the phone. Flame is MIT-licensed and Flutter is free (BSD), and
Flame's current version is 1.37, from spring 2026. (Flame on pub.dev; flame-engine on GitHub.)

FACT: the whole workflow uses Flutter's own command-line tool. You create, run, and build with
commands like these:

```bash
flutter create my_game
flutter run                 # run on a connected phone or emulator
flutter build apk           # or: flutter build appbundle  (Android)
flutter build ipa           # iOS (needs a Mac)
```

(Flutter docs, *CLI reference* and the Android and iOS deployment guides.)

Assessment: this is the strongest pick if mobile is your main target. It is the only tool in
this survey that was built for phones from the start, rather than adding mobile as an
afterthought. It uses one language (Dart), has excellent documentation and an official
Google game codelab, and Flutter's fast reload makes for a quick build-and-see loop. The
trade-off is that it is 2D only and you are learning Dart plus Flutter's way of doing things.

## libGDX

FACT: libGDX is a mature Java framework for 2D and 3D games, cross-platform including Android
and iOS, under the Apache 2.0 license. It has been around since roughly 2013 and is still
active, with version 1.14 released in June 2026. iOS support comes through a separate project
(RoboVM/MobiVM) that turns the Java into an iOS app. (libgdx.com; libGDX GitHub.)

FACT: you start a project with its generator (a downloadable jar called gdx-liftoff), then use
Gradle, the standard Java build tool, from the command line:

```bash
java -jar gdx-liftoff.jar          # generate the project
./gradlew lwjgl3:run               # run on desktop
./gradlew android:installDebug     # build and install on Android
./gradlew ios:launchIPhoneSimulator
```

(libGDX docs, *Project generation* and *Import and running*.)

Assessment: libGDX is free, battle-tested, and backed by a large community, and it does both
2D and 3D. It is heavier to get going than the others, though: it is code-first with no
editor, and you deal with Gradle, a JDK, and the Android SDK yourself, with iOS needing a Mac
and the extra RoboVM piece. It is a great fit for someone already comfortable with Java
tooling, and a rougher start for someone who is not.

## MonoGame

FACT: MonoGame is a free C#/.NET framework for 2D and 3D games, the open-source continuation
of Microsoft's old XNA. It is cross-platform including Android and iOS. Its current version is
3.8.4, from October 2025, and it is actively maintained by a non-profit foundation. One detail
to get right: its license is the Microsoft Public License (Ms-PL), not MIT, though Ms-PL is
also permissive and free. (MonoGame GitHub; MonoGame docs.)

FACT: it uses the standard `dotnet` command-line tool. You install the MonoGame project
templates, scaffold a project, and build:

```bash
dotnet new mgandroid          # or mgios, mgdesktopgl, etc.
dotnet build
```

Building for phones also needs the .NET mobile workloads
(`dotnet workload install android` / `ios`), and iOS still needs a Mac. Game art is compiled
through MonoGame's content tool, `mgcb`. (MonoGame docs, *Platforms*.)

Assessment: MonoGame is a fine, free choice for anyone who knows C#, with a long XNA lineage
and plenty of tutorials. Like libGDX it is code-first with no editor, and its mobile builds
add some setup (the workloads, and Xcode for iOS). If C# is your language, it is the natural
pick.

## HaxeFlixel

FACT: HaxeFlixel is a free, MIT-licensed 2D game framework written in Haxe, built on the
OpenFL and Lime libraries. It exports to Android and iOS. It is actively maintained (the flixel
library reached 6.1 and its Lime build tool 8.3 in late 2025 and 2026). (haxeflixel.com;
Haxe library.)

FACT: everything runs through one command-line tool called `lime`, which is a real
convenience, since the same command shape targets every platform:

```bash
haxelib install flixel        # install it
lime test android             # build and run on Android
lime test ios                 # iOS (needs a Mac)
```

(HaxeFlixel docs, *Install* and *Android*.)

Assessment: HaxeFlixel is free, purpose-built for 2D games, and has a friendly community. Its
single unified `lime` command for every target is genuinely nice. The costs are that you must
learn Haxe, a language you may not have met, and its ecosystem is smaller than the others
here. For a 2D-focused hobbyist who does not mind a less common language, it is a solid,
tidy option.

## Sources

- Flame — https://pub.dev/packages/flame ; https://github.com/flame-engine/flame
- Flutter CLI and mobile builds — https://docs.flutter.dev/reference/flutter-cli ; https://docs.flutter.dev/deployment/android ; https://docs.flutter.dev/deployment/ios
- libGDX — https://libgdx.com/ ; project generation https://libgdx.com/wiki/start/project-generation ; running https://libgdx.com/wiki/start/import-and-running
- MonoGame — https://github.com/MonoGame/MonoGame ; platforms https://docs.monogame.net/articles/getting_started/platforms.html ; license https://github.com/MonoGame/MonoGame/blob/develop/LICENSE.txt
- HaxeFlixel — https://haxeflixel.com/documentation/install-haxeflixel/ ; https://haxeflixel.com/documentation/android/
