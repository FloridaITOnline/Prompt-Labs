# 🧩 Core Primitives of Context Anchoring
*Version 1.3 — GPL v3 Open Framework*  
[![License: GPL v3](https://img.shields.io/badge/license-GPLv3-blue.svg)](./LICENSE)

> **Context Anchoring is a prompt-native computation model built on three core primitives —  
Anchors → Gates → Loops — that enable persistent state, structured reasoning, and controlled iteration, entirely inside an LLM’s context window.**

This document replaces earlier “Seven Atoms” terminology with a more compact, more accurate framework.

---

# 🧭 Conceptual Overview

Traditional prompting treats each turn as isolated.  
Context Anchoring treats prompting as **computation**.

The three core primitives form a closed reasoning cycle:

```

ANCHOR(S) → GATE(S) → LOOP(optional) → updated ANCHOR(S)

```


Everything else — auditing, constraints, intent, compression — is a **behavior applied to these primitives**, not a primitive itself.

This makes Context Anchoring:
- Deterministic
- Modular
- Stateful
- Testable
- Extensible

And critically:
> No fine-tuning.  
> No external database.  
> No hidden memory.  
> Everything occurs **inside** the prompt context boundary (~10 KB).

---

# ⚛️ The Three Primitives

| Primitive | Definition | Analogy | Role |
|-----------|------------|---------|------|
| **Anchor** | A named, persistent context register. | Variable stored in memory | Holds stable state across steps |
| **Gate** | A reasoning step that reads/updates anchors. | Function / transformation | Performs logic or evaluation |
| **Loop** | Optional repetition of Gates until stability. | Iteration / while-loop | Enables progressive refinement |

These three structures form the **core computation graph**.

---

## ✅ 1) Anchor

> **A stable, named reference register that preserves task context across steps.**

Anchors define:
- What remains true
- Scope and boundaries
- Current state
- Shared knowledge
- Operational parameters

Anchors may be:
- Static (objective / domain)
- Dynamic (score, progress)
- Derived (results of Gate output)

They change only when explicitly updated.

Examples:

```

A0 — Objective: 3rd grade weather vocabulary
A1 — Difficulty: Easy
A2 — Domain: Weather terms only
A3 — Progress: {correct: 2, incorrect: 1}

```


Anchor properties:
- Named
- Persistent
- Explicit
- Referencable
- Minimal
- In-bounds definition

Anchors ≈ CPU registers  
> Small but essential units of persistent state.

---

## ✅ 2) Gate

> **A Gate is a bounded reasoning step that reads from Anchors, performs a transformation, and optionally updates Anchors.**

A Gate:
- Consumes Anchor context
- Produces validated output
- Optionally writes updated state back into Anchors

Gates are modular and pipeline-friendly.

A Gate may:
- Generate output
- Evaluate correctness
- Transform structure
- Summarize state
- Enforce contracts

Generic form:

```
Gate(Input Anchors) → Output
```
Or iterative:

```
Gate(A_in) → A_out
```

Gates can nest, chain, or run sequentially.

Gates ≈ Functions  
> They create structured reasoning flows between Anchors.

---

## ✅ 3) Loop (Optional)

> **The Loop repeats Gates until a stop condition is satisfied — stability, teacher command, or exhaustion.**

Loops enable:
- Iterative refinement
- Progressive reasoning
- Controlled repetition

Termination conditions:
- Objective met
- Teacher stop
- Stable output
- Threshold reached

Loops ≈ While-loops  
> They allow controlled iterative computation inside the window.

Not all workflows need loops — many are single-pass.

But loops enable:
- Self-correction
- Progressive summarization
- Multi-stage reasoning

---

# 🔧 Cross-Cutting Behaviors

These are not primitives — they decorate Anchors + Gates.

| Behavior | Role |
|----------|------|
| **Verification (Audit)** | Checks contract adherence, correctness, safety |
| **Constraints** | Define boundaries (domain, structure, safety) |
| **Intent** | Declares objective (often stored as Anchor A0) |
| **Compression** | Summarizes state to fit within context window |
| **State Transfer** | Occurs automatically when Anchors update |

These behaviors may occur:
- At Anchor creation
- Before a Gate
- Inside a Gate
- After a Gate
- At the transition to next Loop

Verification is **contextual**, not primitive.  
It can target:
- Single Anchor
- Multiple Anchors
- Gate output
- Inter-anchor relationships

This gives flexibility without expanding the core primitive set.

---

# 🧠 How They Work Together

       ┌───────────┐
       │  Anchors  │
       │ (State)   │
       └─────┬─────┘
             │ read
             ▼
        ┌─────────┐
        │  Gate   │
        │Compute  │
        └─────┬───┘
              │ write
              ▼
       ┌──────────────┐
       │UpdatedAnchor │
       └─────┬────────┘
             │
             ▼
      (Optional Loop)


This creates a reasoning pipeline with memory.

---

# 🔄 Example

### Step 1 — Anchors

```
A0 — Objective: Weather vocab
A1 — Difficulty: Easy
A2 — Domain: Weather terms
A3 — Score: 0

```


### Step 2 — Gate (generate)
> Read A0–A2 → produce puzzle

### Step 3 — Gate (evaluate)
> Validate student answer → update A3

### Step 4 — Loop
> Repeat until teacher stops

The Anchors maintain continuity across Gates.

---

# 🧬 Comparison With Earlier “Seven Atoms”

Old View → 7 atoms  
New View → 3 primitives + cross behaviors

| Old Atom | New Home |
|----------|----------|
| Intent | Anchor (A0) |
| Constraint | Anchor / Gate contract |
| Gate | Gate |
| Audit | Verification behavior |
| Anchor | Anchor |
| Reinforcement Loop | Loop |
| State Transfer | Anchor update |

This simplification removes redundancy and clarifies relationships.

---

# 🏗️ Molecular Structure (Updated)

> **Anchors = memory nodes**  
> **Gates = reasoning edges**  
> **Loops = repetition cycle**

This is a **graph**, not a stack.

Multiple gates can operate over the same anchor graph.

---

# 📏 Runtime Boundaries

All Anchors + Gates + operational context must fit inside ~10 KB.  

This ensures that:
- The model never loses its working set
- All persistent state is visible
- Deterministic behavior is achievable

> **Runtime limit = stability limit.**

This is a *design constraint* of prompt-native computation.

---

# ✅ Closing Summary

Context Anchoring relies on **three core primitives**:

> **ANCHOR → GATE → LOOP**

- **Anchors** store named, persistent state
- **Gates** execute reasoning over Anchors
- **Loops** iterate Gates until complete

Cross-cutting behaviors like verification, constraint, and compression support these primitives but are not primitive themselves.

This simplification makes the model:
- Easier to teach
- Easier to implement
- More faithful to how prompt-native computation actually works
- More modular and less redundant

---

**Author:** Justin Rodriguez  
**Framework:** Context Anchoring v1.3 — GPL v3 Open Framework  
**Series:** Florida IT Online — Prompt-Labs / Context-Anchoring Core
