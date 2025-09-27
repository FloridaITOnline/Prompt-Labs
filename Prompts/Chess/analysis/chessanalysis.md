# Chess Analysis Contract v1.1.2

## 🎯 Purpose
This document defines the authoritative rules for deterministic chess PGN analysis.  
It specifies **input**, **output**, and the **gated validation system** to ensure correctness and reproducibility.

---

## 📤 Output Contract

### CSV Header (Corrected)
GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort

markdown
Copy code

- **Field fill-policy:**
  - Only fill if **present or deterministically derivable** from PGN.
  - Unknowns remain **blank** (never guess).
- **Determinism:** same PGN → same CSV row.
- **Delimiters:**  
  - Output wrapped in `===CSV===` blocks.  
  - Fields with commas are quoted.  
  - Quotes escaped by doubling.  
- **Stability:** Identical input must always produce identical output.

---

## 🧩 Gate System

Each gate enforces a constraint. You must **pass in sequence** (00 → 08).  
Format per gate: **Purpose → Teach → Learn Question → Expected Answer → Notes.**

---

### Gate 00 — Guides Loaded & Checks Enabled
**Purpose:** Confirm rules from `chessanalysis.md` are loaded.  
**Teach:**  
- Don’t guess; blanks for unknowns.  
- Strict fill-policy.  
- Output must be strictly delimited.  
- Deterministic.  
- Only structured JSON/CSV, no prose.  
**Learn Question:** What are the key constraints?  
**Expected Answer:**  
- No guessing.  
- Strict fill-policy.  
- Use `===STEP1-JSON===` and `===CSV===`.  
- Deterministic.  
- Structured only.  
**Notes:** Fails → stop session.

---

### Gate 01 — PGN Integrity & Tag Extraction
**Purpose:** Validate PGN structure, extract tags.  
**Teach:**  
- Required tags: `Event`, `Site`, `Date`, `Result`, `TimeControl`.  
- Optional: `White`, `Black`, `WhiteElo`, `BlackElo`, `ECO`, `Opening`.  
- Normalize dates to `YYYY.MM.DD`.  
- Strip comments/NAGs/variations.  
- SAN-only moves.  
- Extract GameId from `Site` if possible.  
**Learn Question:** Which tags are required vs optional?  
**Expected Answer:** Required: Event, Site, Date, Result, TimeControl. Optional: White, Black, WhiteElo, BlackElo, ECO, Opening.  
**Notes:** Missing tags → blanks, never inferred.

---

### Gate 02 — Move Legality & FEN Fidelity
**Purpose:** Ensure legal play and correct FEN generation.  
**Teach:**  
- Moves must not leave king in check.  
- SAN disambiguation enforced.  
- Castling rights update correctly.  
- En passant only valid immediately.  
- Halfmove resets on pawn/capture.  
- Fullmove increments after Black.  
- FEN must contain all 6 fields.  
**Learn Question:** What resets the halfmove clock?  
**Expected Answer:** Any pawn move or capture.  
**Notes:** Illegal moves invalidate PGN.

---

### Gate 03 — Score Normalization (White POV)
**Purpose:** Normalize evaluations to White’s perspective.  
**Teach:**  
- Always convert engine evals to White POV.  
- Negative evals (good for Black) become positive scores for Black’s side.  
- Mate scores: + = White mates, – = Black mates.  
- No eval → blank.  
**Learn Question:** If eval = –0.5 (Black better), what is the normalized score?  
**Expected Answer:** +0.5 from White POV.  
**Notes:** Deterministic across runs.

---

### Gate 04 — Opening Recognition
**Purpose:** Identify ECO / Opening / Variation.  
**Teach:**  
- If tags present, use them.  
- Else match first N moves to openings DB.  
- Ambiguity → blank.  
- Never guess.  
**Learn Question:** What do you do if ECO is missing and no match?  
**Expected Answer:** Leave ECO blank.  
**Notes:** Always deterministic.

---

### Gate 05 — Blunder Mantras & Justification
**Purpose:** Enforce tactical explanation for blunders.  
**Teach:**  
- Core mantras: checks/captures/threats, LPDO, overloads, forks, pins, skewers, weak squares, zwischenzug, pawn storms, “one more move.”  
- Forcing-move protocol: enumerate checks → captures → threats.  
- Queen-lift heuristic for mate-nets.  
- Tie-in: numeric blunders = Loss ≥ 200 cp.  
**Learn Question:** How are blunders justified?  
**Expected Answer:** By citing at least one mantra and forcing-move protocol.  
**Notes:** Human-readable justification mandatory.

---

### Gate 06 — Accuracy Metric (Fallback)
**Purpose:** Provide fallback accuracy.  
**Teach:**  
- If no evals, compute:  
accuracy = 1 - (Σ|Δscore_move| / Σ|score_before move|)

yaml
Copy code
- Leave blank if denominator zero.  
- Output real decimal between 0–1.  
**Learn Question:** When is fallback accuracy used?  
**Expected Answer:** Only when no eval context exists.  
**Notes:** Never guess values.

---

### Gate 07 — TimeControl & Metadata Parsing
**Purpose:** Parse timing data.  
**Teach:**  
- Parse `TimeControl` into base + increment.  
- Malformed → blanks.  
- Parse UTCDate, UTCTime, move times if explicitly present.  
- Never infer missing time.  
**Learn Question:** If `TimeControl` is `600+5`, how is it parsed?  
**Expected Answer:** Base=600, Increment=5.  
**Notes:** Only deterministic parsing allowed.

---

### Gate 08 — CSV Contract Conformance
**Purpose:** Validate CSV formatting.  
**Teach:**  
- Header must match spec exactly.  
- Fill only deterministically; blanks otherwise.  
- Strict CSV formatting: quoting/escaping enforced.  
- Output inside `===CSV===`.  
- Deterministic stability.  
**Learn Question:** What must the final CSV block contain?  
**Expected Answer:** Correct header row + one data row, properly delimited, formatted, deterministic.  
**Notes:** Non-conformance = fail.

---

## 🔁 Teach / Learn / Answer Loop
Each gate requires answering its validation Q before proceeding.  
Failure = stop until corrected.

---

## 🌱 Extensions
- Additional gates (e.g. Gate 09+) may be added modularly.  
- Tactical audits, error summaries, or extra metadata supported via append-only fields.  
