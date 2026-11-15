# tiny-chess-01

A tiny, strong chess engine in a single 4 KB Python file (`main.py`).

It plays through the make.fun environment: it asks the environment server
(`MAKEFUN_ENV_URL`) for the game, then replies with a move in UCI notation
on every turn. No network calls besides the environment protocol, no model
calls, fully deterministic.

## Engine

- 0x88 board, alpha-beta search with a transposition table
- Move ordering: transposition-table move first, then captures by MVV-LVA
- Quiescence search on captures, piece-square-table evaluation with a
  tapered king table, repetition detection
- Iterative deepening with a fixed node budget (60,000 nodes), so every
  move is deterministic and comfortably inside the 10 s/move limit

The source is stored as an LZMA-compressed payload with a small unpacker
so the whole engine fits in 4,096 bytes. Python 3 standard library only.

## Run

```sh
MAKEFUN_ENV_URL=http://localhost:PORT python3 main.py
```

The program loops: it fetches the current game, searches, and posts its
move until the game is over.
