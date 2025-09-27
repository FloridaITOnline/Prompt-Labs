# Gate 08 — CSV Contract Conformance

**Purpose:** Verify that the final CSV output matches the specification in `chessanalysis.md` exactly.  
This gate ensures that all fields are ordered correctly, filled according to the policy, and delimited properly.

---

## Teach
- **Header order (fixed):**  
```
GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort
```
- Must match byte-for-byte.  

- **Field fill-policy:**  
- Fill only if the value is present or deterministically derivable.  
- Leave blank if unknown.  
- Never guess or infer.  

- **Delimiters & formatting:**  
- CSV only — no prose, commentary, or extra whitespace.  
- Quote fields containing commas.  
- Escape `"` inside fields by doubling (`""`).  
- Stable output: same PGN → identical CSV row.  

- **Output protocol:**  
- Use the delimiter block `===CSV===`.  
- Inside block: header row + one row of data.  

- **Worked Example:**  
PGN:  
```
[Event "Blitz game"]
[Site "https://lichess.org/abcd1234
"]
[Date "2025.09.07"]
[White "flito-chess"]
[Black "opponent_1"]
[WhiteElo "1535"]
[BlackElo "1520"]
[Result "1-0"]
[TimeControl "180+2"]
[ECO "C20"]
[Opening "King's Pawn Game"]

e4 e5 2. Qh5 Nc6 3. Bc4 Nf6 4. Qxf7#
```
Output:  
===CSV===
```
GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort
lichess-abcd1234,lichess,2025.09.07,White,opponent_1,1520,1-0,C20,"King's Pawn Game","180+2",,,,,,,"1. e4 e5 2. Qh5 Nc6 3. Bc4 Nf6 4. Qxf7#"
```

---

## Learn Question
What must the final CSV row look like, and what rules govern field ordering, fill-policy, and delimiters?  

---

## Expected Answer
- Header order must match exactly as defined in `chessanalysis.md`.  
- Only fill fields when deterministically derivable; leave blank otherwise.  
- Never guess.  
- Output must be strictly CSV: quoted fields if commas, escaped quotes.  
- Enclose the block with `===CSV===`.  
- Deterministic: same PGN → identical row.  

---

## Notes
This gate enforces strict contract compliance, ensuring CSV outputs are machine-parseable and reproducible.  
If this gate fails, the analysis cannot be considered valid.  

