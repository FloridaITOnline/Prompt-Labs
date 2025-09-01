# Technique: Context Anchoring

> Note: This definition of *Context Anchoring* is released into the public domain.  
> It is intended as a shared concept, free for anyone to use, adapt, and build upon.  

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

---

## Notes
- Works well with **chunking** and **outline-driven prompting**.  
- Effective for Copilot-style coding, technical docs, or Q&A pipelines.  
- Serves as a practical, lightweight alternative to fine-tuning.  
