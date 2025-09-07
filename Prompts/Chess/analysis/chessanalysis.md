# Chess Analysis Specification (chessanalysis.md)

**Version:** v1.1.0  
**Purpose:** This document defines the authoritative rules and readiness checks for analyzing a single PGN into a deterministic CSV row.  
It also defines the **Gate System** (nine core gates, expandable) that must be passed before analysis may begin.

---

## Part A — Core CSV Contract

**CSV Header (fixed order):**

GameId,Platform,Date,MyColor,Opponent,OppElo,Result,ECO,Opening,TimeControl,Blunders,Mistakes,Inaccuracies,ACPL,Accuracy,SystemTag,MovesShort

**Completion Policy:**
- Fill only when present or deterministically derivable from PGN.  
- No guessing. Unknown fields → blank.  
- Quote fields with commas, escape `"` by doubling.  
- Emit CSV only (no prose).  
- Determinism: same PGN → identical output.  

---

## Part B — Readiness Gates

Before PGN analysis begins, the system must pass a sequence of gates.  
Each gate is defined in a separate file `...\gateXX.md` (two-digit numbering).  

- **Teach:** Excerpt from the corresponding gate doc (concept, rules, examples).  
- **Learn:** A validation question that must be answered correctly.  
- **Answer:** The expected correct answer, recorded here once the gate is passed.  

> Progress only when a gate passes. If a gate fails, reflect on the error, retry until correct.  
> When all gates are passed, readiness is confirmed and PGN analysis may begin.

---

### Gate 00 — Guides Loaded & Checks Enabled
**Teach:** See `...\gate00.md`  
**Learn:** What constraints govern this analysis (e.g., no guessing, fill policy)?  
**Answer:** [to be filled in after passing]  

---

### Gate 01 — PGN Integrity & Tag Extraction
**Teach:** See `...\gate01.md`  
**Learn:** Which PGN tags are required, which are optional, and how should missing tags be handled?  
**Answer:** [to be filled in after passing]  

---

### Gate 02 — Move Legality & FEN Fidelity
**Teach:** See `...\gate02.md`  
**Learn:** What rules ensure moves are legal when reconstructing FEN (castling, en passant, half/fullmove counters)?  
**Answer:** [to be filled in after passing]  

---

### Gate 03 — Score Normalization (White-POV)
**Teach:** See `...\gate03.md`  
**Learn:** How are centipawn and mate scores normalized to White’s perspective?  
**Answer:** [to be filled in after passing]  

---

### Gate 04 — Opening Recognition
**Teach:** See `...\gate04.md`  
**Learn:** What is the procedure for assigning ECO/Opening values, and when must they be left blank?  
**Answer:** [to be filled in after passing]  

---

### Gate 05 — ACPL & Error Counts
**Teach:** See `...\gate05.md`  
**Learn:** How do you compute ACPL and classify inaccuracies, mistakes, and blunders?  
**Answer:** [to be filled in after passing]  

---

### Gate 06 — Blunder Mantras & Justification
**Teach:** See `...\gate06.md`  
**Learn:** What checklist should be applied to identify and justify blunders?  
**Answer:** [to be filled in after passing]  

---

### Gate 07 — Accuracy Metric (Fallback)
**Teach:** See `...\gate07.md`  
**Learn:** How is FLITO-Accuracy computed from ACPL, and when should it be used?  
**Answer:** [to be filled in after passing]  

---

### Gate 08 — TimeControl & Metadata Parsing
**Teach:** See `...\gate08.md`  
**Learn:** How should common TimeControl formats be normalized?  
**Answer:** [to be filled in after passing]  

---

### Gate 09 — CSV Contract Conformance
**Teach:** See `...\gate09.md`  
**Learn:** What is the exact CSV header order and completion policy?  
**Answer:** [to be filled in after passing]  

---

## Part C — Optional Extensions

- **Additional Gates:** May be added beyond Gate 09 using `...\gate10.md`, `...\gate11.md`, etc.  
- **Optional Modules:** Tactical Audit or other enrichment rules may be defined separately, but must follow the same Teach/Learn/Answer pattern.  

---

## I/O Protocol

- **Input:** A complete PGN (headers + moves), plus optional enrichment flags.  
- **Output:**  
  1. Anchored CSV row (Part A)  
  2. Optional extensions (e.g., Tactical Audit)  
- **No prose** — only structured output blocks.  
