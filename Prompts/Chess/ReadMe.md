# Chess Analysis (Context Anchoring)

Purpose : Demonstrates context anchoring by analyzing chess games.  
** See parent README for technique overview.

⚠️ API Key Safety  
Your account = your responsibility. Never paste keys here.  
Use exports if you prefer. If you’re just experimenting, skip to **Quickstart** below 
and use PGN exports (no keys required).

---

## Quickstart (Auto-Fetch; No Auth)

1. Open **`./quickstart/quickstart.md`** and copy its contents into your assistant’s context window.  
2. Paste your **single-game PGN** when prompted.

> Uses the free Lichess **Cloud Eval** endpoint (no tokens/accounts). Subject to rate limits per IP.  
> Connecting a Lichess account does **not** raise Cloud Eval limits, but it can enable faster authenticated game exports and other features—see **Account Connections**.

---

## Using Other Setups

1. **Pick a setup** from `./setup/`:
   - `noauth-lichess-setup.md` — *no login*, paste PGN, Cloud Eval only.
   - `lichess-account-setup.md` — requires **LICHESS_USERNAME** + **LICHESS_TOKEN**; lets you fetch your games and may allow higher export throughput.
   - `chessdotcom-setup.md` — requires **CHESSCOM_USERNAME** (public data; no token).
   - `chessis-setup.md` — export PGN from the app; no token.

2. **Edit placeholders** (example):
   ```md
   LICHESS_USERNAME: your_username
   LICHESS_TOKEN: your_personal_token_here
```
Do not commit tokens; prefer ephemeral sessions.

Copy into your context window in this order:

the chosen ./setup/...md

./analysis/chessanalysis.md

Run it, then paste a single-game PGN
(or let the account setup fetch games if it supports direct export).

>Tip: If something fails (fetch/rate limits), fall back to paste-mode by exporting PGNs and re-running with noauth-lichess-setup.md.

---

## ⚠️ Account Connections

If you plan to use Lichess, Chess.com, or Chessis with these examples, please see the 
[Account Setup Guide](../account-setup.md).

- [**Lichess Setup**](../lichess/account-lichess-setup.md): Requires a free account + personal API token.  
- [**Chess.com Setup**](../chessdotcom/account-chessdotcom-setup.md): Uses the public API (no login needed for most data).  
- [**Chessis Setup**](../chessis/account-chessis-setup.md): Chessis is an Android app, but can also run on Windows using Google Play.  
  Installation requires a free Google (Gmail) account. The app itself is free.  
  Use the built-in PGN export feature to generate game files for analysis.  

🔑 You are 100% in control of your accounts and connections. This repo never collects, 
stores, or manages your credentials.

---

⚙️ Capability & Fetch Modes (RAG-friendly)

Some assistants can’t call HTTP or read local files. This project supports two modes:

Paste-Mode (default, no keys): Export your games (PGN/JSON) from Lichess / Chess.com / Chessis and paste them below the prompt.

Direct-Fetch Mode (optional): If your assistant supports web calls and you provide an Account Card with consent, it may fetch via the platforms’ published endpoints.

RAG required (for continuity): After normalization, save the emitted CSV/JSONL to your own storage (e.g., Git repo). In later sessions/tools, re-anchor by pasting those rows back into context. That’s your portable “memory” across Copilot, Gemini, ChatGPT, etc.

---

## Next Steps
- [General Account Setup Guide](../account-setup.md)  
- [Example Prompts](../ChessAnalysis.md)  
- Chat about your games! You can export analyses and keep them for your personal portfolio.  
  In another lab, we will demonstrate how to use the output from `ChessAnalysis.md` to track 
  your own portfolio locally in your Git repository.

  ---

  ## License & Usage for /chess

Everything contained under the `/chess` directory is released into the **public domain**.  
This includes all ideas, terminology, concepts (such as *Context Anchoring*),  
and any supporting notes, code, or documentation.  

You are free to use, share, adapt, and build upon this material without restriction,  
for personal, educational, or commercial purposes. No attribution is required,  
though it is always appreciated.  

In short: this is a gift — please use it freely.  

[![License: Public Domain](https://img.shields.io/badge/license-Public%20Domain-brightgreen.svg)](#license--usage-for-chess)


