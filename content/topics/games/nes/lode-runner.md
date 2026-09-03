---
title: "Lode Runner — NES Guide"
type: game-guide
category: games/nes
tags: [nes, nintendo, lode-runner, puzzle, platformer, walkthrough, guide]
platform: NES
year: 1987
created: 2026-09-03
draft: false
---

# Lode Runner (NES)

**Developer:** Hudson Soft | **Publisher:** Broderbund | **Year:** 1987

---

## Introduction

Lode Runner was a hit before the NES existed. Broderbund published the original on Apple II in 1983, and it spread across every personal computer platform of the era. By 1987, when Hudson Soft brought it to the Famicom and NES, the design had already been pressure-tested for four years and proven to be nearly inexhaustible: 150 levels of grid-based puzzle-platforming, each one a contained problem that demands you think before you act.

The premise is simple. You are a stick figure in enemy territory. There is gold on the floor. You need to collect it all and reach the top of the screen. Enemies — Broderbund's manual calls them "monks" — chase you through the level, and you cannot fight them directly. Your only tool is a drill: press a button to dig a hole to your left or right. Enemies fall into holes. Holes fill back in after fifteen seconds. That is the whole system, and 150 levels' worth of interesting problems can be wrung from it.

This is a game about planning. Moving first and thinking second kills you here. The best runs look smooth because the player worked out a route before touching the joystick.

---

## Controls

- **D-Pad:** Move left and right. Climb ladders and ropes automatically by pressing up or down when touching them.
- **A:** Drill right. Digs a hole one tile to your right. Only works on diggable brick floors, not concrete, ladders, or rope.
- **B:** Drill left. Same as above, to your left.
- **Up/Down on ladders:** Climb.
- **Nothing to grab walls or jump.** Lode Runner has no jump. You fall. Plan accordingly.

### Movement Rules

- You move one tile at a time on a grid, but movement is fluid at normal speed.
- Falling into a hole you cannot escape from kills you at the bottom of a shaft.
- You can walk over enemies who are trapped in holes — this is how you survive most levels.
- Rope tiles let you hang and travel laterally. You fall if you let go over empty space.
- Ladders only go up and down. You cannot fall through them unless you are not touching them.

---

## Core Mechanics

### Digging

You can only dig one tile deep in a horizontal floor to your left or right. You cannot dig downward, dig concrete (grey blocks), dig ladders, or dig through air. The hole appears immediately and the floor tile begins refilling after a few seconds. The full refill cycle takes approximately 15 seconds at normal game speed.

A hole you dig gives you a gap to slip through and a trap for enemies. An enemy that walks over an open hole falls in and is stuck while the hole is open. Once the floor refills with an enemy in it, the enemy is carried to the top of the hole — which can be useful for moving enemies to a new position.

**Critical rule:** if you are standing in a hole when it refills, you die. Watch your own holes.

### Enemies

Monks are smarter than they look. They path-find toward you and will climb ladders and ropes to reach you. They can also dig — later levels feature monks who dig holes to cut off your route or to escape holes they fall into.

A trapped monk is safe as long as the hole is open. Once the floor starts to refill, the monk climbs out. Timing your traversal to cross an enemy trap before it closes is a core skill.

Monks cannot be killed. They respawn at the top of the screen if a hole crushes them (refill with an enemy too deep in the ground). You are never reducing the number of enemies — you are managing their positions.

### Gold

Every gold piece on a level must be collected before the exit ladder appears. The ladder drops from the top of the screen once the last piece is collected. It is always at the top of the map. The moment it drops, your only job is to reach it.

Some gold is in plain sight. Some is hidden under floor tiles — you drill the floor and the gold appears. The game does not tell you which tiles are hiding gold, so some levels require drilling everywhere that is diggable.

---

## Strategy

### Read the Level First

Before moving a tile, look at the entire screen. Identify: where is all the visible gold, where might hidden gold be, where are the enemies starting, and what is the only viable route to the ladder. Lode Runner levels almost always have one or two correct routes. Moving randomly finds the wrong one.

### Control Enemy Position

You cannot kill monks, but you can put them where they will not bother you. Dig a hole near them, let them fall in, and move past. On levels where enemies can reach the ladder before you, dig a hole between them and the ladder. On levels where enemies dig and chase, you sometimes need to plan your route around where monks will be three moves from now, not where they are now.

### The Refill Is a Tool

A refilling hole ejects any enemy inside it to the top of the shaft. This is often annoying, but it can also be used deliberately to move a monk to a position where they are no longer blocking your path. On a few levels, this is the solution.

### Hidden Gold

If you have cleared every visible gold piece and the ladder has not appeared, there is hidden gold under a floor tile somewhere. Drill every diggable floor tile you have not touched. Blank concrete tiles cannot hide gold. Brick tiles can. Be systematic.

### Save States

Lode Runner has no in-game saving or passwords. Each level must be completed in one sitting in the original game. Batocera's RetroArch save states solve this. There is no shame in saving before a hard segment and reloading. The design intends that you learn each level, not that you replay the first ninety levels every time you make a mistake at level 91.

---

## Stage Structure

The game has 150 stages. They increase in difficulty roughly linearly, though a few outlier-hard stages appear in the 30–60 range before the difficulty curve expects them. The first ten levels function as a tutorial for the digging mechanic. By level 20, the game assumes you can plan a route and control enemy position. By level 50, it assumes you can plan three moves ahead and use the refill timing deliberately.

There is no credits sequence, no final boss. Level 150 is just another puzzle. After completing it, the game loops.

---

## Tips

- When multiple enemies are chasing you, lead them all into a single hole if possible. A pile of monks in one trap gives you the whole rest of the level to work with.
- Ropes are your best friend in the mid-game. You can hang on a rope above a hole, drill the floor beneath a moving enemy, and drop it while they walk over it — all without moving laterally.
- Learn the difference between concrete and brick. Concrete is undiggable, period. Brick looks slightly different in texture. When your drill does nothing, you are hitting concrete.
- The first few seconds of each level, before enemies path-find to you, are your window to establish position. Use them.

---

## In Brief

| | |
|---|---|
| **Genre** | Puzzle-platformer |
| **Players** | 1 |
| **Save system** | None (use save states) |
| **Difficulty** | Scales to hard by level 40, very hard by level 80 |
| **Completion time** | 6–12 hours for all 150 levels with saves |
| **Recommended for** | Players who liked Sokoban or any puzzle game where thinking beats reflexes |
