---
type: lesson
series: mobile-game-engines
chapter: 3
title: Minimal libraries
status: curated
tags: [software, game-development, mobile, cli, raylib, ebitengine, macroquad, rust]
created: 2026-07-01
---

# Minimal libraries: small, fast, close to the metal

These give you a small, simple game API and little else, no editor, few layers. They are a
joy for programmers who want to understand everything in their project, and they are all free.
The honest trade-off for mobile is that "supports mobile" here usually means the game runs on
the device, but you assemble the actual app package yourself with the platform's own tools
(the Android NDK, Xcode). None of them offers a one-button phone build the way a full engine
does, so I flag where each one is official versus do-it-yourself.

## raylib

FACT: raylib is a very simple C game library for 2D and 3D, under the permissive zlib license.
It is very actively developed, with version 6.0 released in April 2026, and it is widely known
as one of the friendliest libraries for beginners because of its tiny, consistent API.
(raylib.com; raylib GitHub.)

FACT: Android is officially supported, but you build it yourself using the Android NDK. From
raylib's source folder, the make-based path looks like this:

```bash
make PLATFORM=PLATFORM_ANDROID ANDROID_NDK=<ndk_path> \
     ANDROID_API_VERSION=<api_level> ANDROID_ARCH=arm64-v8a
```

There is also a CMake path. (raylib wiki, *Working for Android*.) FACT: iOS is **not** in the
official repo; there is an active community fork (raylib-iOS) and an open pull request, but no
official iOS backend, so treat iPhone support as community-made and experimental. (raylib
GitHub discussions.)

Assessment: raylib is the gold standard for a simple, approachable API, and its C code is easy
to read and learn from. The rough edge for mobile is that the Android packaging is manual and
the iOS story is unofficial. It is a wonderful way to learn game programming and to ship
desktop and Android games; lean elsewhere if iPhone is a must.

## Ebitengine

FACT: Ebitengine (formerly Ebiten) is a dead-simple 2D game library for the Go language, under
the Apache 2.0 license. It is mature and active, with version 2.9 released in March 2026, and
it officially supports both Android and iOS. (ebitengine.org; Ebitengine GitHub.)

FACT: its mobile tool, `ebitenmobile`, is among the cleaner ones here. It does not build a
whole app; it builds a native library you drop into an Android Studio or Xcode shell project:

```bash
go install github.com/hajimehoshi/ebiten/v2/cmd/ebitenmobile@latest
ebitenmobile bind -target android -javapkg your.package -o yourgame.aar .
ebitenmobile bind -target ios -o YourGame.xcframework .
```

(Ebitengine docs, *Mobile*.)

Assessment: Ebitengine is a strong pick if you like Go. The API is deliberately minimal, the
documentation is good, and it is one of the few small libraries with official support for both
Android and iOS. You still need the native toolchains set up, but the mobile step itself is
tidy. For a simple 2D game in a modern, easy language, it is one of the best options on this
whole list.

## macroquad

FACT: macroquad is a simple 2D game library for Rust, dual-licensed MIT or Apache 2.0, and
actively maintained (version 0.4.15, May 2026). It is deliberately small and is often
described as a Rust take on raylib, with fast build times and few dependencies. (macroquad
GitHub; crates.io.)

FACT: for Android, macroquad leans on its lower-level backend and a Docker image so you do not
have to install the Android NDK yourself:

```bash
docker run --rm -v $(pwd):/root/src -w /root/src \
  notfl3/cargo-apk cargo quad-apk build --release
```

FACT: iOS is manual and fiddly. There is no dedicated tool; you cross-compile with cargo, hand
-assemble the app bundle, and install it through Xcode's tools. (miniquad GitHub;
macroquad.rs.)

Assessment: macroquad is a pleasant way to make a small 2D game in Rust, and much lighter than
the bigger Rust engine below. Its Android path via Docker is clever and avoids a painful NDK
setup, but its iOS support is genuinely do-it-yourself, so again, favor another tool if iPhone
matters.

## A note on Bevy

FACT: Bevy is the larger, more serious Rust game engine, doing both 2D and 3D with a
data-driven design, dual-licensed MIT or Apache 2.0, and free. Its current version is 0.19,
from June 2026, and it is still before its 1.0 release, with breaking changes every few
months. Mobile support exists and is improving: Android builds through a tool called
`cargo-ndk`, and iOS through a Makefile and Xcode project in its examples. (bevy.org; Bevy
GitHub.)

Assessment: Bevy is powerful but is a full engine with a real learning curve, and being before
1.0 it is a moving target that can break your code between versions. It is worth knowing about
if you are set on Rust and want more than macroquad offers, but it is not a gentle starting
point. For a first Rust game, start with macroquad; grow into Bevy if you outgrow it.

## Sources

- raylib — https://github.com/raysan5/raylib ; Android build https://github.com/raysan5/raylib/wiki/Working-for-Android ; iOS discussion https://github.com/raysan5/raylib/discussions/2681
- Ebitengine — https://github.com/hajimehoshi/ebiten ; mobile https://ebitengine.org/en/documents/mobile.html
- macroquad — https://github.com/not-fl3/macroquad ; Android via miniquad https://github.com/not-fl3/miniquad ; iOS https://macroquad.rs/articles/
- Bevy — https://github.com/bevyengine/bevy ; mobile examples https://github.com/bevyengine/bevy/blob/main/examples/README.md
