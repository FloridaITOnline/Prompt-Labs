# Gate 01 — PGN Integrity & Tag Extraction

**Purpose:** Ensure that PGN headers (tags) are parsed correctly and handled according to strict no-guessing rules.  
This gate establishes how metadata is extracted before any move analysis begins.

---

## Teach
- **Required tags:**  
  - `Event`  
  - `Site` (often includes game ID or platform)  
  - `Date`  
  - `Result`  
  - `TimeControl`  

- **Optional tags (fill if present, else blank):**  
  - `White`, `Black` (player names)  
  - `WhiteElo`, `BlackElo` (ratings)  
  - `ECO` (opening code)  
  - `Opening` (opening name)  

- **Handling missing tags:**  
  - Never guess or infer from context.  
  - If a tag is absent or malformed, leave its field **blank**.  

- **Parsing rules:**  
  - Strip comments, NAGs (`$1`, `$2`…), and variations `( ... )`.  
  - SAN moves only — don’t mix headers and moves.  
  - Normalize date format to `YYYY.MM.DD`.  
  - Extract game ID from `Site` if possible (e.g., `https://lichess.org/abcd1234` → `lichess-abcd1234`).  

- **Determinism:** The same PGN always produces the same extracted tags.  

---

## Learn Question
Which PGN tags are required, which are optional, and how should missing tags be handled during extraction?  

---

## Expected Answer
- Required tags: `Event`, `Site`, `Date`, `Result`, `TimeControl`.  
- Optional tags: `White`, `Black`, `WhiteElo`, `BlackElo`, `ECO`, `Opening`.  
- Missing tags: leave blank, never guess.  
- Parse only SAN moves; ignore comments, NAGs, and variations.  
- Normalize dates to `YYYY.MM.DD`.  
- Extract game ID from `Site` when possible.  
- Same PGN → same tags (deterministic).  

---

## Notes
This gate ensures that metadata extraction is strict, repeatable, and never relies on inference.  
If this gate fails, later gates will produce unstable or incorrect CSV fields.  
