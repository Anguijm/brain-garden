---
type: lesson
series: godot
chapter: 3
title: GDScript
status: curated
tags: [software, godot, game-development, gdscript, scripting, beginner-friendly]
created: 2026-06-30
---

# GDScript

So far your nodes just sit there. A script is what makes a node do something: move, react,
keep score. Godot's main language for this is GDScript, and this chapter covers enough of
it to read and write simple scripts.

## What GDScript is

FACT: GDScript is a programming language built specifically for Godot. It is object-oriented
and uses a clean, indentation-based style very similar to Python, though it is its own
separate language. (Godot docs, *GDScript basics*.) Assessment: if you have seen Python it
will look familiar, and if you have not, it is one of the gentler languages to start with,
because it was designed to fit the engine and stay readable.

FACT: GDScript is not your only option, Godot also supports C# and, through an add-on system
called GDExtension, languages like C++. (Godot docs, *Scripting languages*.) Assessment: but
GDScript is the default, and every official beginner tutorial uses it, so it is the right
place to start. You can always add another language later.

## A script attaches to a node

This is the key link back to [chapter 1](01-nodes-and-scenes). A script does not float on
its own; it attaches to a node and extends what that node can do.

FACT: the first line of almost every script is `extends`, naming the type of node the script
is attached to. Writing `extends Sprite2D` means the script gains all the abilities of a
`Sprite2D`. You attach a script by right-clicking a node in the Scene dock and choosing
"Attach Script." (Godot docs, *Creating your first script*.)

```gdscript
extends Sprite2D

func _ready():
    print("Hello from the robot")
```

That tiny script, attached to a `Sprite2D`, prints a message when the game starts. (We meet
`_ready()` properly in the [next chapter](04-interactivity); for now, read it as "run this
when the game begins.")

## The handful of pieces you need

Assessment: you can read most simple Godot scripts knowing just a few building blocks.

- **Variables** hold values, and you make one with `var`, for example `var speed = 200`. A
  variable is just a labeled box you can put a number, a word, or an object into.
- **Functions** are named blocks of steps, and you make one with `func`. The code inside a
  function runs when something calls it.
- **Constants** are values that never change, made with `const`, like `const MAX_LIVES = 3`.

FACT: GDScript can work out the type of a value on its own, or you can state it plainly with
a colon, as in `var speed: int = 200`. (Godot docs, *GDScript basics*.) Assessment: stating
the type is optional at the start, but it helps the editor catch mistakes for you, so it is
a good habit to grow into.

## Exported variables: settings you can tune

Here is a small feature that does a lot of work. FACT: putting `@export` in front of a
variable makes it show up in the Inspector, so you can change its value right there in the
editor without touching the code, and the value gets saved with the scene. (Godot docs,
*GDScript exported properties*.)

```gdscript
extends Sprite2D

@export var speed = 200   # now adjustable in the Inspector

func _ready():
    print("My speed is ", speed)
```

Assessment: this is how you keep code and tuning separate. A programmer writes the behavior
once, and then anyone, including future you, can tweak the numbers in the Inspector to make
the game feel right, no code editing required.

FACT: one Godot 3-versus-4 gotcha to know, because it breaks so many old tutorials: today
you write `@export` with the `@` sign, but Godot 3 used a bare `export` with no `@`. (Godot
docs, *GDScript exported properties*.) If you copy an older script and it errors on
`export`, that missing `@` is usually why.

## The takeaway

Assessment: a script is `extends` a node type, plus some variables and functions that give
it behavior, with `@export` for the values you want to tune by hand. That is the shape of
nearly every script you will write. The [next chapter](04-interactivity) puts scripts in
motion: the game loop, reading the player's input, and signals.

## Sources

- Godot docs, *GDScript basics* — https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html
- Godot docs, *Creating your first script* — https://docs.godotengine.org/en/stable/getting_started/step_by_step/scripting_first_script.html
- Godot docs, *GDScript exported properties* — https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_exports.html
- Godot docs, *Scripting languages* — https://docs.godotengine.org/en/stable/tutorials/scripting/index.html
