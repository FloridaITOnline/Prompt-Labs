# Gate 07 — TimeControl & Metadata Parsing

**Purpose:** Standardize how PGN `TimeControl` values (and related metadata) are parsed and normalized into the CSV output.  
This gate ensures consistent handling of time formats across platforms.

---

## Teach
- **TimeControl formats (PGN standard):**  
  - `300+0` → 300 seconds (5 minutes) base time, 0 second increment.  
  - `180+2` → 180 seconds (3 minutes) base, 2 second increment per move.  
  - `600+5` → 600 seconds (10 minutes) base, 5 second increment.  
  - `1/10` → Correspondence game with 1 move per 10 days.  

- **Friendly aliases (non-standard cases):**  
  - Some PGNs may contain descriptive strings like `bullet`, `blitz`, or `correspondence`.  
  - If such tags appear, normalize to blank unless explicitly mapped to a standard format.  

- **Normalization rule:**  
  - Preserve canonical `base+increment` or `X/Y` format when possible.  
  - Leave blank if unparsable. Never guess or invent.  

- **Other metadata checks:**  
  - Strip whitespace around values.  
  - Ensure determinism: the same input always maps to the same normalized string.  

- **Worked Examples:**  
  - Input: `300+0` → Output: `300+0`  
  - Input: `180+2` → Output: `180+2`  
  - Input: `bullet` → Output: `` (blank)  
  - Input: `1/10` → Output: `1/10`  

---

## Learn Question
How should common TimeControl formats (e.g., `300+0`, `180+2`, `1/10`) be represented, and what should be done with non-standard aliases like `bullet`?  

---

## Expected Answer
- Canonical formats (`base+increment` or `X/Y`) should be preserved as-is.  
- Non-standard aliases like `bullet` should be left blank unless explicitly mapped.  
- Never guess or invent values.  
- Same input → same normalized output (deterministic).  

---

## Notes
This gate ensures that time control values are always consistent in the CSV.  
By rejecting non-standard or ambiguous formats, it upholds the “no guessing” principle while still allowing for common PGN cases.  
