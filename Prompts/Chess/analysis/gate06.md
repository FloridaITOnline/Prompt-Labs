# Gate 06 — Accuracy Metric (Fallback)

**Purpose:** Define how to calculate a proxy accuracy score when platform-provided accuracy (e.g., Chess.com) is unavailable.  
This gate ensures consistency in reporting accuracy values derived from ACPL.

---

## Teach
- **Primary rule:**  
  - If platform accuracy is provided (e.g., from Chess.com PGN tags), use it directly.  
  - If no accuracy is available, compute **FLITO-Accuracy v0.1** from ACPL.  

- **Formula:**  
Accuracy_color = clamp(100 − α · ACPL_color, 0, 100)
- `α` (alpha) is a scaling constant. Default: **0.04**.  
- `clamp` means restrict the result to the 0–100 range.  

- **Properties:**  
- Monotonic: Higher ACPL → lower accuracy.  
- Tunable: Changing `α` adjusts sensitivity but not the principle.  
- Proxy only: Not intended to reproduce Chess.com’s hidden formula exactly.  

- **Worked Example:**  
- White ACPL = 50  
  - Accuracy_white = 100 − 0.04 × 50 = 98.  
- Black ACPL = 300  
  - Accuracy_black = 100 − 0.04 × 300 = 88.  
- If ACPL = 3000, Accuracy = 100 − 0.04 × 3000 = −20 → clamp to 0.  

---

## Learn Question
How do you compute FLITO-Accuracy when platform accuracy is absent, and what role does the clamp function play?  

---

## Expected Answer
- Formula: Accuracy_color = clamp(100 − 0.04 × ACPL_color, 0, 100).  
- If platform accuracy exists, use it; otherwise compute with ACPL.  
- Clamp ensures results stay between 0 and 100.  
- Example: ACPL 50 → 98, ACPL 300 → 88, ACPL 3000 → 0.  

---

## Notes
This gate ensures the system can always report an accuracy score, even if the platform does not provide one.  
The FLITO-Accuracy v0.1 metric is transparent, tunable, and reproducible across different runs.  

