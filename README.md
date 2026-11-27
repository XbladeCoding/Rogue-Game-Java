# Rogue Game (Java)

A chase game on a grid: a monster hunts the rogue (`@`), and the rogue tries to survive as long as possible. Both sides play themselves using graph search (BFS). The monster always takes the shortest path toward the rogue, and the rogue runs to wherever the monster needs the most moves to reach, preferring spots with more escape routes so it doesn't get cornered.

The idea comes from **Rogue**, a 1980 dungeon game that basically started the whole RPG genre. The full rules and backstory are in [game.md](RogueButBetter/game.md).

This repo also has the rest of my Java coursework (see [What's in here](#whats-in-here)).

## How to run
You need [Java](https://adoptium.net/) (JDK 17 or newer).

```bash
cd RogueButBetter
javac *.java
java Game
```

`Game`'s `main` runs a short test game on a small dungeon and then prints all 5 built-in dungeons. To try a different dungeon or more turns, edit `main` in `Game.java`.

## The dungeon
| Symbol | Meaning |
|---|---|
| `@` | Rogue |
| `A`-`Z` | Monster |
| `.` | Room (you can move in 8 directions, diagonals included) |
| `+` | Corridor (only 4 directions: N, E, S, W) |
| ` ` (space) | Wall |

The monster moves first. If it lands on the rogue, the game is over (for the rogue, anyway).

**Manual mode:** you can also control the rogue yourself with `W`/`E`/`S`/`Q` for N/E/S/W, `A`/`D`/`Z`/`C` for the diagonals and `X` to stay put.

## How the code works
| File | What it does |
|---|---|
| `Site.java` | A position on the grid `(i, j)`, plus Manhattan distance |
| `Dungeon.java` | Stores the grid and figures out which moves are legal |
| `Game.java` | Runs the game loop, displays the board and has 5 test dungeons |
| `Monster.java` | Chase strategy: BFS to find the shortest path and take the first step |
| `Rogue.java` | Escape strategy: BFS from the monster to score every move, or manual keyboard control |

We built this with pair coding, one piece at a time. [implementation.md](RogueButBetter/implementation.md) has our step-by-step plan.

## What's in here
| Folder | What it is |
|---|---|
| `RogueButBetter/` | The Rogue game (latest version, with the docs) |
| `Rogue/` | An earlier copy of the same game |
| `Java code/` | Class exercises (loops, arrays, methods and so on), plus two bigger projects: `Java Final/` (an early version of my dungeon crawler) and `JavaFinalFinal/` (an F1 racing game built with StdDraw) |
| `Recursion Problems/` | Classic recursion practice: binary search, Tower of Hanoi, subset sum and more |
| `python stuff/` | A small Python script (sort of a side quest) |
