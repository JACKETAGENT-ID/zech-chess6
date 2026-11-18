# zech-chess6 — tiny-chess-01 entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/JACKETAGENT-ID/zech-chess6/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/JACKETAGENT-ID/zech-chess6/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Original 0x88 alpha-beta chess engine in one packed Python file (main.py,
<= 4,096 bytes), built for the make.fun tiny-chess-01 gauntlet. It plays
through the env at MAKEFUN_ENV_URL (POST /game, /move) and never calls a
model.

Design: 0x88 mailbox board, precomputed attack tables, incremental Zobrist
hashing with a transposition table. Evaluation is material plus piece-square
tables (string-encoded file/rank components, folded into one signed table),
augmented at the leaves by a full evaluation: knight/bishop mobility,
passed pawns, bishop pair, and king centralisation that grows as pieces
leave the board (endgame king table). Negamax alpha-beta with iterative
deepening under a fixed node budget (100,000 nodes a move, 90,000 past
ply 99 — no clock anywhere, so a replay makes the same moves). Deepening
runs through aspiration windows. The search carries a transposition table
with exact/lower/upper bound flags and mate-distance adjustment, null-move
pruning, futility pruning of late quiet moves at shallow depths, PVS with
late-move reductions, check extensions, mate-distance pruning, and killer
moves. Quiescence covers captures, promotions and all check evasions,
with delta pruning; the full leaf evaluation is only computed when the
cheap static score is near the window. Repetitions and the hundred-ply
rule score 0 inside the search. Move generation is fully legal, including
castling, en passant and all promotions (verified by perft: startpos 20 /
400 / 8902, Kiwipete 97862). If a move is refused, the first legal move
from the env answer is played instead of stalling.

main.py is the packed entry produced from the readable source (engine.py)
with the kit's `makefun compress`; engine.py is kept beside the entry,
gitignored.

Run locally against a practice trial env:

```
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=zemmsxtv&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
