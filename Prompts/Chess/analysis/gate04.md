# Gate 04 — Opening Recognition (ECO/Name)

**Purpose:** Ensure that openings are recognized correctly from PGN data.  
This gate establishes when ECO/Opening values can be trusted, when dictionary matching is allowed, and when fields must remain blank.

---

## Teach
- **Primary source:**  
  - Use `ECO` and `Opening` PGN tags if present and valid.  

- **Fallback (dictionary match):**  
  - If tags are missing or malformed, attempt to match the first 8–12 plies against a small embedded ECO dictionary.  
  - Dictionary includes only top-level opening families (e.g., Ruy Lopez, Italian, Sicilian, French, Caro-Kann, Queen’s Gambit, Slav, King’s Indian, Nimzo-Indian, Benoni, English, London).  
  - Transpositions must still match the early move sequence reliably.  

- **Blank policy:**  
  - If no confident match is found, leave both ECO and Opening blank.  
  - Never guess or invent.  

- **Determinism:**  
  - Same PGN → same ECO/Opening assignment (tags or dictionary).  

---

## Learn Question
What is the procedure for assigning ECO/Opening values, and when must they be left blank?  

---

## Expected Answer
- Use ECO/Opening tags from PGN if valid.  
- If tags are missing or corrupted, check the first 8–12 plies against a limited ECO dictionary.  
- If no confident match, leave ECO/Opening blank.  
- Never guess or invent values.  
- Always produce the same result for the same PGN (deterministic).  

---

## Notes
This gate ensures opening information is handled conservatively and consistently.  
It prevents mislabeling of openings and keeps unknown cases blank, which preserves the “no guessing” principle of the spec.  
