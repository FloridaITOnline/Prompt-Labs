# Gate 00 — Guides Loaded & Checks Enabled

**Purpose:** Confirm that the authoritative guide (`chessanalysis.md`) is loaded and its constraints are understood before any analysis begins.  
This is the handshake between the orchestrator and the spec.

---

## Teach
- The authoritative rules live in **`chessanalysis.md`**.  
- **No guessing:** Values must only be filled when present or deterministically derivable. Unknowns must remain blank.  
- **Field fill-policy:** Follow exactly as specified (e.g., OppElo from opponent tag only, never inferred).  
- **Delimiter rules:** All outputs must use the fixed CSV header order and block delimiters (`===STEP1-JSON===`, `===CSV===`).  
- **Determinism:** Re-running the same PGN must produce identical output (aside from stable whitespace in MovesShort).  
- **No prose:** Only structured outputs (JSON, CSV). No commentary.  

---

## Learn Question
What are the key constraints that govern this analysis according to `chessanalysis.md`?  
*(Hint: no guessing, strict field-fill policy, delimiter rules, determinism, no prose.)*

---

## Expected Answer
- Do not guess — unknowns must remain blank.  
- Follow strict field fill-policy as defined in `chessanalysis.md`.  
- Outputs must be strictly delimited (`===STEP1-JSON===`, `===CSV===`).  
- Determinism: same PGN always → same output.  
- Only structured outputs (JSON or CSV), no prose.  

---

## Notes
Passing this gate confirms that the system recognizes the **rules of engagement** for all subsequent gates.  
If this gate fails, the session should stop until corrected.  
