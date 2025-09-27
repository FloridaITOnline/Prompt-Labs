# Gate 05 — ACPL & Error Counts

**Purpose:** Teach how to compute Average Centipawn Loss (ACPL) for each side and classify inaccuracies, mistakes, and blunders based on evaluation differences.  
This gate ensures error metrics are derived consistently and deterministically from evaluation data.

---

## Teach
- **Evaluation basis:**  
  - Use centipawn (cp) scores normalized to White’s POV (see Gate 03).  
  - For each move, measure the evaluation *before* and *after* the move from the mover’s perspective.  

- **Loss formula:**  
  - Let `cp_before` = evaluation before the move (mover’s POV).  
  - Let `cp_after_for_mover` = `- cp_before_next_ply` (since the next ply is the opponent’s perspective).  
  - Compute:  
    `Loss = max(0, cp_before − cp_after_for_mover)`  

- **Worked Example:**  
  - Suppose just before White’s move, `cp_before = +40`.  
  - The next ply (after White moves, it’s Black to move) shows `cp_before_next_ply = +20`.  
  - So `cp_after_for_mover = −20`.  
  - Loss = max(0, 40 − (−20)) = 60.  
  - Since 60 ≥ 50 but < 100, this counts as an **Inaccuracy**.  

- **ACPL (Average Centipawn Loss):**  
  - For each color, ACPL = average of all Loss values across that side’s moves.  
  - Round or truncate consistently; must be stable across runs.  

- **Classification thresholds (per move):**  
  - Inaccuracy: Loss ≥ 50 cp  
  - Mistake: Loss ≥ 100 cp  
  - Blunder: Loss ≥ 200 cp  

- **Counts:**  
  - Maintain per-game counts of inaccuracies, mistakes, and blunders for both sides.  

- **Determinism:**  
  - Identical evaluations → identical ACPL and counts.  

---

## Learn Question
How do you compute ACPL for each side, and what thresholds define inaccuracies, mistakes, and blunders?  

---

## Expected Answer
- ACPL = average of `Loss` across all moves for a color.  
- Loss = max(0, cp_before − cp_after_for_mover), where cp_after_for_mover = − cp_before_next_ply.  
- Thresholds:  
  - Inaccuracy ≥ 50 cp  
  - Mistake ≥ 100 cp  
  - Blunder ≥ 200 cp  
- Track counts of each category.  
- Deterministic: same evaluations always produce same ACPL and counts.  

---

## Notes
This gate anchors the link between raw engine evaluations and human-readable error metrics.  
It ensures ACPL and error counts are reproducible and aligned with well-known chess analysis standards.  
