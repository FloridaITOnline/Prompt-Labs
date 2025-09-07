# Lichess Login & Context Load — AI Prompt (Fetch or Paste + Normalize)

> Copy this entire prompt into your AI assistant.  
> 1) Fill in the **ACCOUNT CARD**.  
> 2) If `mode: paste`, you can paste later using the RAW INPUT block (see below).  
> 3) Send.  
> This step **loads normalized context only**. Run analysis afterward with `.../Chess/ChessAnalysis.md`.  
> **Security:** Do not store or log the `username` and/or the `token` beyond this context window.

---

## ACCOUNT CARD (fill before sending)

> **Compatibility Note**  
> To maximize reliability across different AI platforms and keep uploads small, this lab targets games up to **15 minutes** (rapid/blitz). Longer time controls can create larger payloads and hit limits. You can raise this if your assistant supports bigger payloads.

=== ACCOUNT CARD ===
engine: lichess
username: <YOUR_LICHESS_USERNAME>
mode: fetch | paste
auth:
type: bearer | none
token: <PASTE_TOKEN_IF_YOU_ACCEPT_THE_RISK_AND_HAVE_HTTP; ELSE LEAVE BLANK>
limits:
max_games: 10
output:
format: pgn # pgn | ndjson | json (source format; normalization happens here)
include_moves: true
include_opening: true
enrichment:
strategy: critical # or every-n (captured here; enrichment runs in the analysis step)
max_positions: 5
min_cp_swing: 120
notes:
consent: I authorize this session to use the above solely to fetch my Lichess data via the published API.
=== END ACCOUNT CARD ===

---

## RAW INPUT (deferred — paste later when convenient)

> You do **not** need to paste anything now. When you’re ready (later in this same chat), reply with your data wrapped exactly like this:
>
> ```
> === RAW INPUT ===
> <PASTE YOUR PGN / NDJSON / JSON HERE>
> === END RAW INPUT ===
> ```
>
> The assistant will detect this block when it appears later and proceed automatically.

---

## Normalization Schema (must match exactly)

Between delimiters, one game per block. Use `null` for unknowns. SAN must be single-line; strip comments `{}`, NAGs `$n`, and side-variations `( … )`. If no stable ID is present, use `pgn-<YYYYMMDD>-<index>`.

=== GAME ===
GameId: <source-id-or-stable-fallback>
Source: lichess
Event: <string|null>
Site: <url|null>
Date: <YYYY.MM.DD|null>
White: <name> (<?elo?>)
Black: <name> (<?elo?>)
Result: <1-0|0-1|1/2-1/2|*>
TimeControl: <e.g., 180+2|null>
ECO: <code|null>
Opening: <name|null>
Termination: <string|null>

Moves:
<single-line SAN, no comments/variations/NAGs>
=== END GAME ===


---

## Operating Rules (follow strictly)

- **Capability Gate**
  - If HTTP/web tools are available **and** `mode: fetch`, use **Direct-Fetch**.
  - Otherwise (no HTTP **or** `mode: paste`), allow **deferred paste**:
    - If a `=== RAW INPUT === ... === END RAW INPUT ===` block is **present in this message**, use it.
    - If no RAW INPUT block is present **yet**, do **not** error—emit `=== FETCH PLAN ===` and `=== AWAITING RAW INPUT ===` (see Output Contract) and wait for a later message containing the RAW INPUT block.

- **Direct-Fetch (Lichess API)**
  - Base: `https://lichess.org/api`
  - User games: `GET /api/games/user/{username}?max={max}&moves={include_moves}&opening={include_opening}`
  - If `output.format = ndjson` → set `Accept: application/x-ndjson`
  - If `output.format = pgn` (default) → plain-text PGN is fine
  - If `output.format = json` (single game) and you have IDs → `GET /game/export/{gameId}?accuracy=1`
  - **Auth:** when `auth.type = bearer` and `token` present → `Authorization: Bearer <token>`
  - **Rate limits:** single request at a time; on **429** wait **60s**, retry **once**; keep `max_games ≤ 10`

- **Normalization**
  - Convert **either** fetched data **or** RAW INPUT (when provided) to the **Normalization Schema** above.
  - Remove `{}` comments, `$n` NAGs, and `( … )` side-variations.
  - Flatten SAN to one line; preserve check/mate markers (`+`, `#`) if present.
  - Derive `GameId` from `Site/Link` when possible; else use the schema’s fallback.

- **Security**
  - Never print the token or auth headers.
  - Do not store credentials; discard after the request.
  - Summaries must not reveal secrets.

---

## Output Contract (strict — no extra prose)

Produce **exactly** these sections, in order:

0) Auth step
   - Ask the user for their lichess 'username' and thier Token. Do not save this information. Ask for it everytime you need to query the Lichess Endpoint.

2) `=== FETCH PLAN ===`
   - `mode` (fetch|paste)  
   - `username`  
   - `http_available` (true|false)  
   - `output.format` (pgn|ndjson|json)  
   - `max_games`  
   - `auth.used` (true|false; do not print token)

3) One of the following:
   - **Deferred paste path (no RAW INPUT yet):**  
     Emit `=== AWAITING RAW INPUT ===` with the instruction:
     ```
     Reply later with your data wrapped exactly as:

     === RAW INPUT ===
     <PGN / NDJSON / JSON>
     === END RAW INPUT ===
     ```
     *(Do not emit NORMALIZED CONTEXT yet.)*
   - **Data present path (fetched or pasted now):**  
     `=== NORMALIZED CONTEXT ===`  
     - One or more **GAME** blocks conforming to the schema (and nothing else).

4) `=== NEXT STEP ===`  
   - `Ready for analysis. Proceed with ChessAnalysis.md using the normalized context above.`
  
   

---

## Ready Check (execute now)

- Parse the **ACCOUNT CARD**.
- Decide **fetch** vs **paste**:
  - If `mode: fetch` **and** HTTP is available → fetch, then normalize.
  - If `mode: paste` **and** RAW INPUT block is present → normalize it.
  - If `mode: paste` (or no HTTP) **and** no RAW INPUT yet → emit `FETCH PLAN` + `AWAITING RAW INPUT` and wait.
- Emit the sections exactly as specified above.
