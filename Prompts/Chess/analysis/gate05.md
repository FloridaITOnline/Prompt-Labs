# Gate 05 — Blunder Mantras & Justification

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

- **Forcing-Move Protocol (mandatory):**  
  1. Enumerate **all checks** (Q, R, B, N, P, castle).  
  2. Enumerate **all captures**.  
  3. Enumerate **direct threats** (mate nets, hanging pieces, forks).  
  4. For each candidate, give one reason it might work (or why it fails).  
  5. If a forcing move is blocked on one square, **scan sibling squares** for the same motif (e.g., if …Qb6 is blocked by Bc5, also test …Qh4 / …Qh5 for threats on f2/h2).  

- **Queen-Lift Heuristic (special case):**  
  - If a bishop targets **c5/c4** (or b4/b5 analogues) and a knight eyes **g4/g5/e4/e5**,  
    always test **Qh4, Qh5, Qb3, Qb6** (and mirrors) for mate nets on **f2/f7/h2/h7**.  
  - If the king just moved (Kg1/Kg8), **re-run queen-lift scan immediately** — king moves often activate these nets.  

- **Connection to Gate 05:**  
  - Blunders are flagged numerically when Loss ≥ 200 cp.  
  - This gate explains *why* it is a blunder with one or more mantras.  

- **Worked Example:**  
  - PGN fragment:  
    ```
    [Event "Casual"]
    [Site "https://example.org"]
    [Date "2025.09.07"]
    [White "PlayerA"]
    [Black "PlayerB"]
    [Result "*"]

    1. e4 e5 2. Nf3 Nc6 3. Bc4?? Nd4
    ```
  - Here, 3. Bc4?? is flagged as a blunder because after 3…Nd4, Black attacks both c2 and f3 with tempo.  
  - **Mantra applied:** Fork patterns → Knight fork (Nf3+ and Nc2+ threats).  
  - Justification: “Blunder — Fork pattern: Knight on d4 creates dual threats, losing material.”  

- **Anchor Test — Queen-Lift Net:**  
  - Scenario: After White plays **Kg1**, Black has bishop on c5 and knight ready to hit g4/e4.  
  - **Protocol run:** Check queen moves …Qb6 (blocked) and …Qh4 (open).  
  - **Expected result:** …Qh4! threatens mate on h2/f2.  
  - **Mantra justification:** “Forcing queen-lift: …Qh4! creates immediate mate threat.”  

---

## Learn Question
What checklist of mantras should be applied to explain a blunder, and how would you justify the example move 3. Bc4?? in the sample PGN?  
Additionally, what protocol ensures that queen-lift threats (e.g., …Qh4) are not overlooked?  

---

## Expected Answer
- Checklist: checks/captures/threats, attackers vs defenders, LPDO, overload, pins/skewers, forks, back-rank/weak squares, zwischenzug, pawn storms near king, “one more move” rule.  
- Example justification: 3. Bc4?? fails the fork pattern mantra — Black’s Nd4 attacks c2 and f3 simultaneously, creating a decisive fork and losing material.  
- Forcing-move protocol ensures all checks/captures/threats are enumerated, including queen lifts.  
- Queen-lift heuristic specifically requires testing Qh4/Qh5/Qb3/Qb6 in bishop+knight structures, especially after king moves.  

---

## Notes
This gate ensures blunders are not treated as “black boxes.”  
Every flagged blunder must be explained with at least one mantra, and forcing-move protocols must prevent overlooking tactical resources like queen lifts.  
