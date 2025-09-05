# Anchored Chess Analyzer — v1.1.0 (CSV Core + Optional Tactical Audit)

**Goal:** Produce a *deterministic* one-row CSV for a single PGN, exactly as in v1.0.0.  
**Optional:** If `TacticalAudit=true`, also emit a second, separate CSV block for conservative, rule-based tactical flags.

---

## Part A — Anchored Portfolio CSV (unchanged, required)

**Output:** exactly **one CSV block** with this header (order fixed):

GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort

**Rules (condensed, same as v1.0.0):**
- Fill **only** if present or deterministically derivable from PGN tags:
  - GameId: from `Site` lichess URL → `lichess-<id>`; else `pgn--`
  - Platform: `lichess` (or domain from `Site`)
  - Date: `[Date]` → `YYYY.MM.DD`
  - MyColor/Opponent: compare username to `White`/`Black`
  - OppElo: Elo of the opponent (integer)
  - Result: from `[Result]`
  - ECO/Opening/TimeControl: from tags *only*
  - Blunders/Mistakes/Inaccuracies/ACPL/Accuracy: **only** if present in source/enrichment
  - SystemTag: optional passthrough
  - MovesShort: first 6–10 plies (SAN only; strip comments/NAGs/variations)
- **No guessing.** Unknown → blank.  
- **CSV only.** No prose before/after. Quote fields containing commas; escape `"` by doubling.

---

## Part B — Tactical Audit (optional, separate contract)

**When enabled:** If and only if the user provides `TacticalAudit=true`, output **a second CSV block** *after* Part A (one blank line between blocks).

**Audit header (order fixed):**

AuditMoveNo,Side,SAN,Issue,Evidence,MateIn

**Issue enum (strict, conservative, no engine needed):**
- `MateIn2Observed` — The game ends in mate **within the next 2 plies** after this move (i.e., immediate tactical collapse observed in the actual PGN, not hypothetical).
- `DropsMajorPieceNextPly` — After this move, the opponent’s **very next ply** captures a **major/minor piece** (Q/R/B/N) *without* this move being a capture or check that wins back equal material on the following **immediate** ply.
- `OverlooksImmediateMateThreat` — Side to move plays a quiet/irrelevant move and the opponent mates on the very next ply.

**Fields:**
- `AuditMoveNo`: integer move number at which the *flagged* move was played.
- `Side`: `White` or `Black` (the side that made the flagged move).
- `SAN`: the SAN of the flagged move.
- `Issue`: one of the three enum values above.
- `Evidence`: ≤ 12 words, e.g., `... Qh4# next ply` or `... Nxe5 next ply`.
- `MateIn`: integer if `MateIn2Observed` (2 or 1), else blank.

**No guessing policy (audit):**
- Only flag **if the pattern is visible in the provided move sequence** (actual game continuation).  
- Do **not** invent “recommended lines”.  
- If there are **no flags**, output exactly the string: `NoTacticalIssues` as the entire second block (no header).

**Output order when audit is enabled:**
1) Part A CSV (one block)  
2) **Blank line**  
3) Part B CSV **or** `NoTacticalIssues`

---

## I/O Protocol (both parts)
- Input: one complete PGN (headers + moves) and optional `SystemTag`.  
- Output: **CSV only** (and optional second block). No prose.

---

## Anchor Tests (must pass without changing rules)

### A. CSV Anchor (same as v1.0.0)

**Canonical**
[Event "rated blitz game"]
[Site "https://lichess.org/abcd1234
"]
[Date "2025.08.31"]
[White "flito-chess"]
[Black "opponent_1"]
[WhiteElo "1535"]
[BlackElo "1520"]
[Result "1-0"]
[TimeControl "180+2"]
[ECO "C20"]
[Opening "King's Pawn Game"]

e4 e5 2. Bc4 Nc6 3. Qh5 Nf6 4. Qxf7#

**Expected CSV**

GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort
lichess-abcd1234,lichess,2025.08.31,White,opponent_1,1520,1-0,C20,"King's Pawn Game","180+2",,,,,,,"1. e4 e5 2. Bc4 Nc6 3. Qh5 Nf6 4. Qxf7#"


**Edge A (missing headers)**


[Event "rated blitz game"]
[Site "https://lichess.org/efgh5678
"]
[Date "2025.09.01"]
[Result "0-1"]
[TimeControl "180+2"]

e4 e5 2. f3?? Qh4#

**Edge B (draw / low tactics)**

[Event "casual blitz"]
[Site "https://lichess.org/ijkl9012
"]
[Date "2025.09.02"]
[White "flito-chess"]
[Black "opponent_2"]
[Result "1/2-1/2"]
[TimeControl "180+2"]

Nf3 Nf6 2. g3 g6 3. Bg2 Bg7 4. O-O O-O 5. d4 d5 6. c4 c6 7. Nc3 dxc4 8. Ne5 Nd5 9. Nxc4 Nxc3 10. bxc3 Be6 11. Nd2 Bd5 1/2-1/2

**Expected CSV**

GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort
lichess-ijkl9012,lichess,2025.09.02,White,opponent_2,,1/2-1/2,,,,,"180+2",,,,,,,"1. Nf3 Nf6 2. g3 g6 3. Bg2 Bg7 4. O-O O-O"


---

### B. Tactical Audit Anchor (when `TacticalAudit=true`)

Use the same three PGNs above. After emitting **Part A**, emit **Part B** as follows:

**Canonical → expected audit (example pattern):**

AuditMoveNo,Side,SAN,Issue,Evidence,MateIn
4,Black,Nf6,MateIn2Observed,"Qxf7# next ply",1

*(Rationale: after 3...Nf6, the very next White ply delivers mate — observed mate within 1 ply ⇒ count as `MateIn2Observed` with MateIn=1.)*

**Edge A (missing headers) → audit still allowed (based on moves only):**

AuditMoveNo,Side,SAN,Issue,Evidence,MateIn
2,White,f3,DropsMajorPieceNextPly,"... Qh4# next ply",,

*(Here we flag the catastrophic move because the opponent mates **next ply**. If you prefer to classify mate-only cases strictly as `OverlooksImmediateMateThreat`, use that enum instead; keep it consistent in your tests.)*

**Edge B (drawish) → expected audit:**

NoTacticalIssues


**Pass criteria (audit):**
- Uses only the three enumerated issues.
- Patterns are **visible in the actual PGN** (no hypothetical search lines).
- If no rows, output exactly `NoTacticalIssues`.

---

## Determinism & Formatting
- Two blocks max: **CSV core** (+ optional audit).  
- One blank line between blocks.  
- No other text.  
- Re-running on the same PGN yields identical bytes (aside from whitespace in `MovesShort` which must be stable).

---

## Versioning
- v1.1.0 keeps the **CSV contract identical** to v1.0.0.  
- Tactical Audit is **opt-in** and **separately anchored**.  
- If you change audit rules, bump **v1.1.x** and re-run the three audit anchor tests.

