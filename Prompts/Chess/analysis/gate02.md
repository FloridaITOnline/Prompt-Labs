# Gate 02 — Move Legality & FEN Fidelity

**Purpose:** Ensure that moves parsed from PGN are applied legally and that each resulting position is expressed as a valid FEN string.  
This gate enforces chess rules (king safety, castling, en passant, counters) to guarantee fidelity of state reconstruction.

---

## Teach
- **General legality rule:**  
  - Never apply a move that leaves your own king in check.  

- **SAN interpretation:**  
  - Handle disambiguation (e.g., `Nbd2` vs `Nd2`).  
  - Recognize captures (`x`), checks (`+`), mates (`#`).  
  - Handle castling (`O-O`, `O-O-O`).  
  - Handle promotions (e.g., `e8=Q`).  

- **Castling rights:**  
  - Lost when king moves or rook moves.  
  - Lost if rook is captured.  

- **En passant:**  
  - En passant target square exists only immediately after a pawn makes a two-square advance.  
  - Capture by en passant legal only if the pawn is adjacent and captures onto the target square.  

- **Halfmove / fullmove counters:**  
  - Halfmove counter resets on any pawn move or capture.  
  - Fullmove number increments after Black’s move.  

- **FEN parts (6 fields, space-separated):**  
  1. Piece placement (ranks 8 → 1).  
  2. Side to move (`w` or `b`).  
  3. Castling rights (`KQkq` or `-`).  
  4. En passant target square (e.g., `e3` or `-`).  
  5. Halfmove clock (for 50-move rule).  
  6. Fullmove number.  

- **Determinism:** Each move must yield a unique, valid FEN.  

---

## Learn Question
What rules ensure moves are legal when reconstructing FEN (including castling rights, en passant, and half/fullmove counters)?  

---

## Expected Answer
- A move is legal only if it does not leave the mover’s king in check.  
- Disambiguate SAN properly; recognize captures, checks, mates, castles, promotions.  
- Update castling rights when king/rook moves or rook is captured.  
- En passant target set only after a two-step pawn move; capture allowed only if legal onto that square.  
- Reset halfmove counter on pawn moves or captures; increment fullmove counter after Black’s move.  
- Construct full FEN string with 6 fields: placement, side to move, castling, en passant, halfmove, fullmove.  
- Same moves → same FEN (deterministic).  

---

## Notes
Passing this gate ensures the analysis engine produces valid chess positions after each move.  
If FENs are not constructed faithfully, subsequent evaluation and CSV generation will be invalid.  
