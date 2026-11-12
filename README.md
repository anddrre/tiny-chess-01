# tiny-chess-01

A 4 KB chess engine in Python 3.13 (standard library only, deterministic, no network, no model calls).

`main.py` is the packed entry: the engine source is compressed with LZMA and base85-encoded into a two-line stub that decompresses and executes it at run time. The unpacked source lives in `chess-dev/engine.py` (development copy, not part of the entry).

## Run

```
MAKEFUN_ENV_URL=http://127.0.0.1:9 python3.13 main.py
```

The engine speaks the make.fun environment protocol (JSON over HTTP) and plays with a fixed node limit.

## Engine

- Negamax with alpha-beta, quiescence search and a transposition table (262,144 entries).
- Move ordering: captures first (MVV-LVA), then killers/history, with a bonus for promotions.
- Evaluation: material plus piece-square tables.

## v3 (2026-11-12)

- Fixed the transposition-table store mask on cutoff (`262143` instead of `65535`), so cutoff entries are actually reused.
- Added a promotion bonus to move ordering in the main search and in quiescence.

## Verification

- `test_perft.py`: all positions pass.
- `bench.py` (20,000 nodes): same best move as v2 on all five test positions; endgame node count down from 3,685 to 2,782.
- Trial 2 vs the house engines: 5 wins, 0 draws, 11 losses, about 1,173 Elo vs the 1,360 sunfish baseline — below the prize baseline, reported honestly.
