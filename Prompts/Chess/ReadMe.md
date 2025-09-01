# Chess Analysis (Context Anchoring)

Purpose : Demonstrates context anchoring by analyzing chess games.  
** See parent README for technique overview.

⚠️ API Key Safety  
Your account = your responsibility. Never paste keys here.  
Use exports if you prefer. If you’re just experimenting, skip to **Quickstart** below 
and use PGN exports (no keys required).

---

## Quickstart (No API Key Needed)
1. Export games (Lichess / Chess.com / Chessis)  
2. Map them into a context block  
3. Run the analysis prompt  

*(If you later want to connect accounts directly, see **Account Connections** below.)*

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


