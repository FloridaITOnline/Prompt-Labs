# Gate 06 — Blunder Mantras & Justification

**Purpose:** Teach the tactical checklist (“mantras”) that should always be applied when evaluating whether a move is a blunder.  
This gate ensures that flagged blunders are not just detected numerically (via Gate 05) but also explained using human-readable tactical reasoning.

---

## Teach
- **Core Mantras (apply every time):**  
  1. Scan forcing moves first: checks, captures, threats.  
  2. Count attackers vs defenders on the target square.  
  3. LPDO (Loose Piece Drop-Off): don’t leave pieces undefended.  
  4. Overload: don’t assign one defender too many jobs.  
  5. Pins & Skewers: watch discovered lines after your move.  
  6. Fork patterns: Knight forks (N–Q/R), Pawn forks (esp. in time scrambles).  
  7. Back-rank and weak dark/light squares near the king.  
  8. Zwischenzug (in-between forcing moves).  
  9. Pawn storms near your own king: don’t create fatal holes.  
  10. “One more move” rule: always check the opponent’s strongest reply.  

- **Connection to Gate 05:**  
  - Blunders are first flagged numerically (Loss ≥ 200 cp).  
  - This gate requires explaining *why* it is a blunder using one mantra.  

- **Worked Example:**  
  - PGN fragment:  
    ```
    [Event "Casual"]
    [Site "https://lichess.org/example"]
    [Date "2025.09.07"]
    [White "PlayerA"]
    [Black "PlayerB"]
    [Result "*"]

    1. e4 e5 2. Nf3 Nc6 3. Bc4?? Nd4
    ```
  - Here, 3. Bc4?? is flagged as a blunder because after 3…Nd4, Black attacks both c2 and f3 with tempo.  
  - **Mantra applied:** Fork patterns → Knight fork (Nf3+ and Nc2+ threats).  
  - Justification: “Blunder — Fork pattern: Knight on d4 creates dual threats, losing material.”  

---

## Learn Question
What checklist of mantras should be applied to explain a blunder, and how would you justify the example move 3. Bc4?? in the sample PGN?  

---

## Expected Answer
- Checklist: checks/captures/threats, attackers vs defenders, LPDO, overload, pins/skewers, forks, back-rank/weak squares, zwischenzug, pawn storms near king, “one more move” rule.  
- Example justification: 3. Bc4?? fails the fork pattern mantra — Black’s Nd4 attacks c2 and f3 simultaneously, creating a decisive fork and losing material.  

---

## Notes
This gate ensures blunders are not treated as “black boxes.”  
Every flagged blunder must have a tactical explanation linked to a mantra, making results both explainable and reproducible.  
