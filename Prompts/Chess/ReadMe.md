# Everything in Prompts is Experimental

**What is this for:** 
We use Prompt labs to build, test, expirement, and to show history for attempts. Feel free to browse these ideas, but they may not work as we had intended. They may also require additional compute, or in some cases even crafted input. Have fun exploring!

# Chess Analysis (Context Anchoring)

**Purpose:** Demonstrates *Context Anchoring* by analyzing chess games.  
See [../docs/techniques/context_anchoring.md](../docs/techniques/context_anchoring.md) for the full overview and definition of the technique.

---

## Quickstart (Auto-Fetch; No Auth)

1. Open **`./quickstart/quickstart.md`** and copy its contents into your assistant’s context window.  
2. Paste your **single-game PGN** when prompted.  

That’s it — the orchestration prompt handles guide fetching, readiness checks, and analysis.  

---

## Using Other Setups

This repo is modular. You can choose from setups under `./setup/` depending on your environment:

- **`noauth-setup.md`** — paste a PGN directly, no accounts required.  
- **`account-setup.md`** — shows how to configure account-based exports if you prefer to fetch games directly.  
- **`custom-setup.md`** — for experimenting with your own pipelines or external evaluation engines.  

**Important:**  
- Edit placeholders where needed (username, token, etc.).  
- Do not commit any sensitive data (tokens, keys).  
- Ephemeral sessions are preferred for testing.  

---

## Capability & Fetch Modes (RAG-friendly)

Some assistants can’t call HTTP or read local files. This project supports two approaches:

- **Paste-Mode (default, no keys):**  
  Export your games as PGN from any platform and paste them below the prompt.    

**RAG for continuity:**  
After analysis, save the emitted CSV/JSONL to your own storage (e.g., Git repo). In later sessions or tools, re-anchor by pasting those rows back into context. That becomes your portable “memory” across ChatGPT, Gemini, Copilot, etc.  

---

## Next Steps

- [Context Anchoring Guide](../docs/techniques/context_anchoring.md)  
- [Chess Analysis Contract](./analysis/chessanalysis.md)  
- [Example Prompts](./quickstart/quickstart.md)  

You can export analyses and keep them for your personal portfolio.  
In future labs, we’ll demonstrate how to track your portfolio locally in a Git repo.  

---

## License & Usage for `/chess`

Everything in the `/chess` directory is released into the **public domain**.  
This includes all ideas, terminology, concepts (such as *Context Anchoring*),  
and any supporting notes, code, or documentation.  

You are free to use, share, adapt, and build upon this material without restriction,  
for personal, educational, or commercial purposes. No attribution is required,  
though it is always appreciated.  

In short: this is a gift — please use it freely.  

[![License: Public Domain](https://img.shields.io/badge/license-Public%20Domain-brightgreen.svg)](#license--usage-for-chess)
