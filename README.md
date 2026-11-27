# Rogue Game (Java)

A chase game on a grid: a monster hunts the rogue (`@`), and the rogue tries to survive as long as possible. Both sides play themselves using graph search (BFS). The monster always takes the shortest path toward the rogue, and the rogue runs to wherever the monster needs the most moves to reach, preferring spots with more escape routes so it doesn't get cornered.

## Background
In the late 1970s, Ken Arnold built **curses**, a C library that could put a character anywhere on the screen (very crude graphics, but graphics). Michael Toy and Glen Wichman used it to make **[Rogue](https://en.wikipedia.org/wiki/Rogue_(video_game))**: you go down into the Dungeons of Doom, fight monsters, grab treasure and come back up with the Amulet of Yendor. It shipped with BSD Unix in 1980, took over college campuses and basically started the whole RPG genre (NetHack is its direct descendant).

In 1984, grad students at CMU wrote **Rog-o-matic**, a program that played Rogue by itself (and at one point it was the best Rogue player at CMU). Its key trick was staying away from the monster long enough for its health to regenerate. That turns into a graph search problem, which is what this project is about.

## How to run
You need [Java](https://adoptium.net/) (JDK 17 or newer).

```bash
cd Rogue
javac *.java
java Game
```

`Game`'s `main` runs a short test game on a small dungeon and then prints all 5 built-in dungeons. To try a different dungeon or more turns, edit `main` in `Game.java`.

## Rules
The game runs on an N-by-N grid:

| Symbol | Meaning |
|---|---|
| `@` | Rogue |
| `A`-`Z` | Monster (each letter is a different monster) |
| `.` | Room (you can move in 8 directions, diagonals included) |
| `+` | Corridor (only 4 directions: N, E, S, W) |
| ` ` (space) | Wall (no going through these) |

- The monster moves first, then the rogue. Each turn, a player either stays put or moves to an adjacent site.
- If the monster lands on the rogue, the game is over (for the rogue, anyway).
- Rooms never connect directly to each other, so any path between two rooms goes through at least one corridor.
- There is exactly one monster and one rogue, and both start in a room.

**Manual mode:** you can also control the rogue yourself. In `main`, change `new Rogue(game)` to `new Rogue(game, true)`, then use `W`/`E`/`S`/`Q` for N/E/S/W, `A`/`D`/`Z`/`C` for the diagonals and `X` to stay put.

### Custom dungeons
`Game.fromUserInput()` reads a dungeon from the console: first the size N, then N rows of the grid. For example:
```
5
 +++ 
+.@.+
+...+
+.M.+
 +++ 
```

## Strategies

**Monster (chase):** run BFS from the monster to find the shortest path to the rogue, then take the first step. This isn't always perfect (when two shortest paths tie, one can be better than the other), but it never wastes a move.

**Rogue (escape):** run BFS from the *monster* to find how many moves it needs to reach every cell. Then, for each legal move, the rogue picks the one that:
1. is the farthest from the monster (in monster moves, not straight-line distance),
2. has the most escape routes (so it doesn't run into a corner), and
3. doesn't move toward the monster, as a tiebreaker.

**Weakness:** the rogue only looks one move ahead. The best possible strategy finds a loop of rooms and corridors (a cycle in the graph) and runs around it forever. Ours doesn't explicitly search for cycles, so in a dead-end dungeon like `DUNGEON_TRAP` the monster still catches it (in about 6 turns). In the other 4 test dungeons, the rogue survived all 100 turns we tested.

## How the code works
| File | What it does |
|---|---|
| `Site.java` | A position on the grid `(i, j)`, plus Manhattan distance |
| `Dungeon.java` | Stores the grid and figures out which moves are legal |
| `Game.java` | Runs the game loop, displays the board and has 5 test dungeons |
| `Monster.java` | Chase strategy (BFS shortest path) |
| `Rogue.java` | Escape strategy, or manual keyboard control |

We built this with pair coding, one piece at a time.
