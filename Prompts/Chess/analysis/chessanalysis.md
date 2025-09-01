## Portfolio CSV — Output Contract (strict)

> Emit **only** one CSV block with a single header line and one row per game.
> Leave unknowns blank; do not add commentary or extra lines.

**Header (exactly this order):**
GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort

**Field rules**
- GameId: stable id (e.g., lichess-abc123); from normalized context.
- Platform: `lichess`
- Date: `YYYY.MM.DD`
- MyColor: `White` | `Black`
- Opponent: username (or blank)
- OppElo: integer (or blank)
- Result: `1-0` | `0-1` | `1/2-1/2` | `*`
- ECO: code like `C20` (or blank)
- Opening: short name (or blank)
- TimeControl: e.g., `180+2`
- Blunders/Mistakes/Inaccuracies: integers (blank if unknown)
- ACPL: float (blank if unknown) — **use 1 decimal place**
- Accuracy: number **0–100** (no `%`; blank if unknown)
- SystemTag: optional user tag (e.g., your custom system name)
- MovesShort: very short SAN snippet (e.g., first 6–10 plies), no comments

**Example (header + 2 rows)**
GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort
lichess-abcd1234,lichess,2025.08.31,White,opponent_1,1535,1-0,C20,"King's Pawn Game","180+2",,0,1,12.3,98.1,"My-English","1. e4 e5 2. Bc4 Nc6 3. Qh5..."
lichess-efgh5678,lichess,2025.08.30,Black,opponent_2,1540,0-1,B00,"Uncommon Opening","180+2",1,1,,32.5,,,"1. f3 e5 2. g4 Qh4#"

**Constraints**
- Exactly one CSV block. No prose before/after.
- Quote fields that contain commas (e.g., Opening names).
- **If a field contains a double quote, escape it by doubling it** (CSV rule), e.g., `He said ""wow""`.
- Do not include engine details or FENs here—keep it compact for tracking.
- Use data only from the **normalized context** (and any enrichment available in-thread).

## Field Completion Policy (No Guessing)

Always emit the full CSV header. For each game, fill values **only** if they are
(a) explicitly present in the normalized context or enrichment, or
(b) deterministically derivable by simple rules below.
Otherwise leave the cell **blank**. Do not infer from general knowledge.

### Allowed deterministic derivations
- GameId: use from normalized block; if missing, use fallback `pgn-<YYYYMMDD>-<index>`.
- Platform: literal `lichess` (or source from normalized block if multi-platform).
- Date: from PGN/JSON tag `Date` (normalized form `YYYY.MM.DD`).
- MyColor: **case-insensitive** compare: if `username` equals `White` → `White`; if equals `Black` → `Black`; else blank.
- Opponent: the *other* player’s username if `MyColor` is known; else blank.
- OppElo: `WhiteElo` or `BlackElo` corresponding to Opponent (integer only).
- Result: from tag (`1-0`, `0-1`, `1/2-1/2`, `*`).
- ECO / Opening: use **only** if present in normalized block (or enrichment explicitly provided them). Do **not** guess from move text.
- TimeControl: from tag (e.g., `180+2`).
- Blunders / Mistakes / Inaccuracies / ACPL / Accuracy: fill **only** if provided by the source export or the enrichment step; else blank.
- SystemTag: user-provided tag if present; else blank.
- MovesShort: first 6–10 plies from `Moves` (strip comments/NAGs/variations).

### Never infer
- Do not derive ECO/Opening by “recognizing” moves unless an explicit opening map is provided in-context.
- Do not synthesize ratings, accuracy, ACPL, or error counts unless present in export/enrichment.
- Do not reformat dates from outside the provided tags; if malformed, leave blank.

### Validation
- If a value is not present or computable by the rules above, leave it **blank**.
- Do not add commentary in the CSV block.
