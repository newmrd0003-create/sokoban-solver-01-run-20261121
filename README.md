# Sokoban Solver — push-optimal, room by room

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/newmrd0003-create/sokoban-solver-01-run-20261121/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/newmrd0003-create/sokoban-solver-01-run-20261121/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A Sokoban solver that answers every instance of a set push-optimally. One
program, `main.ts`, reads each `/input/<id>/level.txt` and writes
`/output/<id>/solution.txt` with the player's moves in LURD.

## How it works

A level is a hallway with independent rooms off it (3–4 boxes each, one door
onto the hallway). A box pushed out of a door can never come back, so every
room is its own problem and the level's push-optimum is the sum of the rooms'
optima. Each room is solved by breadth-first search over pushes, with two
prunings that never cut a solvable state:

- **dead squares** — a box standing on one can never reach a goal (found by
  pulling back from the goals over open floor);
- **frozen 2×2 blocks** of walls and boxes with a box off its goal.

A state is the boxes' squares plus the player's reachable region (named by its
lowest square), so no position is ever re-expanded. The search is pure and
deterministic: fixed expansion caps (work, never the clock), fixed
tie-breaking, no randomness — two runs write the same bytes.

## Practice set score

Scored with the kit's own scorer on the practice set: **1,000,000 / 1,000,000**
— every instance solved in exactly its push-optimal count:

| instance | boxes | pushes | P\* |
| -------- | ----- | ------ | --- |
| i001 | 3 | 13 | 13 |
| i002 | 4 | 16 | 16 |
| i003 | 8 | 29 | 29 |
| i004 | 12 | 47 | 47 |
| i005 | 16 | 60 | 60 |

Whole practice set solves in ~1.5 s (60 s wall budget for the set).

## Run it

```sh
mkdir -p out
MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

Node 22+, no dependencies, no network. `main.ts` is plain TypeScript with
erasable syntax only, run as-is by Node's type stripping.
