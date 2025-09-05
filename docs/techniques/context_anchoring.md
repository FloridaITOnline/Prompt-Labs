# Technique: Context Anchoring

> Note: This definition of *Context Anchoring* is released into the public domain.  
> It is intended as a shared concept, free for anyone to use, adapt, and build upon.  

---

## Definition
**Context Anchoring** is a prompting technique for maintaining consistency in multi-step AI workflows.  
It involves reusing a previously validated output as a **stable anchor** in subsequent prompts, ensuring the  
AI remains aligned with the intended format, context, or reasoning path before moving forward.  

Unlike few-shot prompting (which provides static examples), Context Anchoring leverages *dynamic examples*  
generated during the same workflow, reinforcing them until the model reliably reproduces the same output.  

---

## Why It Works
- AI models predict language based on immediate context.  
- By reintroducing a validated output, you narrow the prediction space and stabilize responses.  
- This reduces drift across long documents, datasets, or iterative Q&A sessions.  

---

## Method
1. **Generate a seed output** (e.g., a validated row, doc fragment, or paragraph).  
2. **Re-anchor** the model by starting the next prompt with the exact seed output.  
3. **Repeat until stable** — if the AI drifts, re-emphasize the anchor until it matches consistently.  
4. **Advance** once alignment is reached, anchoring each new step to the last known-good output.  

---

## Example
Imagine generating rows of a structured table:  

- **Seed Prompt:**  
  “Generate row 5 of this table: …”  
- **Validated Output (anchor):**  
  `Row 5: [Correct Data]`  
- **Next Prompt (anchored):**  
  “Row 5: [Correct Data]  
   Now generate row 6 in the same format.”  

If the model drifts, reinsert Row 5 until it outputs Row 6 consistently.  

---

## Use Cases
- Long-form document generation (ensuring section-by-section consistency).  
- Tabular or dataset creation from prompts (reduce format drift).  
- Maintaining narrative or reasoning flow in multi-step Q&A.  
- Compliance or schema-driven outputs (JSON, YAML, policy docs).  
- Chess/game analysis portfolios (consistent annotations across PGNs).  

---

## Testing Anchors
To ensure an anchor is *stable*, run a **3-pass test**:  

1. **Canonical Example** – proves the core logic works.  
2. **Edge Case A** – probes robustness at a boundary (e.g., missing field, odd input).  
3. **Edge Case B** – checks invariance under variation (e.g., different shape of input).  
4. *(Optional)* **Negative Example** – forces a fail to confirm error handling.  

The anchor is only considered **locked** once all three pass in sequence without changing the preset rules or schema.  

---

## Edge Case Taxonomy
When designing tests, draw from these categories:  
- **Missing/nullable fields** (omissions, nulls, empty strings)  
- **Type surprises** (numbers-as-strings, lists vs scalars)  
- **Format drift** (alternate date/time, locale, chess headers)  
- **Scale extremes** (very short vs. very long inputs)  
- **Ambiguity** (multiple valid resolutions)  
- **Adversarial noise** (irrelevant or distracting text)  

---

## Meta Documentation
Every anchor benefits from a lightweight companion note:  

1. **Intent & Scope** – what problem it solves, what’s out-of-scope.  
2. **Success Criteria** – what a “PASS” means in practice (schema fidelity, determinism, explainability).  
3. **Edge Categories Covered** – which from the taxonomy are included and why.  
4. **Risk Register** – tricky inputs or assumptions.  
5. **Change Policy** – when it’s okay to update the logic/schema; how to bump versions.  
6. **Ownership** – who maintains the anchor; where logs & regression suites live.  

---

## Notes
- Works well with **chunking** and **outline-driven prompting**.  
- Effective for Copilot-style coding, technical docs, or Q&A pipelines.  
- Serves as a practical, lightweight alternative to fine-tuning.  
- Anchors should be versioned and regression-tested like code.  

---

[![License: Public Domain](https://img.shields.io/badge/license-Public%20Domain-brightgreen.svg)](#license--usage-for-chess)
