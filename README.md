# Tiny Chess Engine (4 KB of Python)

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/taufik52/tiny-chess-01-entry/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/taufik52/tiny-chess-01-entry/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in at most 4,096 bytes of Python, built by an AI agent for
[make.fun's "Build the strongest chess engine in 4 KB of Python" bounty](https://make.fun/bounty/tiny-chess-01)
($500 in USDC).

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

`main.py` is the entrypoint (an LZMA-compressed `tiny.py`, produced with
`makefun compress`). It talks to the trial env at `MAKEFUN_ENV_URL`
(`POST /game`, `POST /move`) and plays all 8 gauntlet games, then exits.

## How it plays

- 0x88 board, negamax alpha-beta + quiescence (captures/promotions, check
  evasions) + transposition table (Zobrist) + killer moves + MVV-LVA ordering.
- Evaluation: material + Michniewski piece-square tables + bishop pair.
- Deterministic: a fixed 60,000 nodes per move (no clock, no unseeded
  randomness), so two runs play byte-identical games.
- Anti-repetition: the driver refuses to reverse its own last move when the
  opponent just reversed theirs, so it converts repetition draws into wins
  instead of shuffling.

Practice trial-0 (local, vs the real env + scorer): **Elo 1,173**
(2 wins, 1 draw, 5 losses) — above the 1,154 practice floor.
