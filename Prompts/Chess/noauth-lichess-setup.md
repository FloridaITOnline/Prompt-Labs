# Lichess Cloud Eval — No Auth (Free)

**Step 1 of 2** → produces normalized analysis data that your `chessanalysis.md` (Step 2) will format.  
This specific step does **not** require API tokens, authentication, servers, or local engines.

---

## What this is
A guide & protocol for using Lichess’s free, unauthenticated **Cloud Evaluation** endpoint to evaluate positions derived from a pasted PGN.

## What this isn’t
- No API keys  
- No login  
- No servers or local engines  

---

## Endpoint (no login needed)

```http
GET https://lichess.org/api/cloud-eval?fen=<URL-ENCODED-FEN>[&multiPv=N]
```

Returns engine evaluations for a single FEN: centipawn or mate score, depth, and PV lines.

## Minimal Flow (PGN → FENs → Cloud Eval → JSON hand-off)

User pastes PGN into your interaction window.

Parse the PGN and reconstruct the position after each full move
(or every ply if you need finer granularity).

For each position, build a legal FEN (standard chess) and call Cloud Eval.

Normalize the response into your Step-1 JSON (schema below).

Pass the JSON to chessanalysis.md (Step 2) for CSV/summary formatting.

💡 Evaluating every full move roughly halves requests while staying useful.

---

## Request Samples
```
GET https://lichess.org/api/cloud-eval?fen=rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR%20b%20KQkq%20-%200%201
GET https://lichess.org/api/cloud-eval?fen=<FEN>&multiPv=3
```

multiPv (optional) asks for top-N lines. Parse defensively if the API returns more/less lines than requested.

---

## Example Response (typical)
```
{
  "fen": "<same fen>",
  "depth": 20,
  "pvs": [
    { "moves": ["e2e4","c7c5","..."], "cp": 23 },
    { "moves": ["d2d4","d7d5","..."], "cp": 18 }
  ]
}
```

A PV includes either "cp" (centipawns, side-to-move perspective) or "mate" (mate in N).

You must normalize to White POV in your pipeline (positive = better for White).

---

## Step-1 Output (normalized JSON) → Input to chessanalysis.md
White POV normalization rule

If the score is reported for the side to move:

White to move → whitePOV = cp (or mate as +N)

Black to move → whitePOV = -cp (or mate as -N)

## Recommended Step-1 JSON Row
```
{
  "ply": 12,
  "fullmove": 6,
  "moveSAN": "Nf3",
  "fen": "r1bqkbnr/pppp1ppp/2n5/4p3/4P3/5N2/PPPP1PPP/RNBQKB1R b KQkq - 2 3",
  "sideToMove": "b",
  "depth": 20,
  "score": { "type": "cp", "raw": 23, "whitePOV": -23 },
  "pvUci": "c7c5 Nc3 Nc6 Bb5 ...",
  "pvSan": "… c5 Nc3 Nc6 Bb5"
}
```

Keep whitePOV as integer centipawns or signed mate per the rule above.

Converting pvUci → pvSan is optional (but helpful for readability).

Cloud Eval does not provide ACPL, Accuracy %, or mistake counts → leave those blank in chessanalysis.md unless added by a separate enrichment step.

---

## FEN Correctness (important)

Use standard chess FEN (not variant FENs like crazyhouse).

Ensure en passant, castling rights, and move counters are valid.
Incorrect EP fields are a common cause of failures.

Example “quick check” list for each FEN:

Castling rights update after any king/rook move.

EP square set only immediately after a 2-step pawn advance and only if capturable.

Half-move/full-move counters increment correctly.

---

## Rate Limits & Etiquette

Lichess applies rate limiting to keep the service responsive:

Make one request at a time (no parallel floods).

On HTTP 429, wait ~60s before resuming.

Add polite pacing (e.g., 120–200 ms delay with small jitter) between calls.

Prefer analyzing every full move to reduce volume.

If you burst requests, rate-limit errors will persist—throttle and back off.

## Practical guidance

Evaluate every full move (halves call count).

Insert ~120–200 ms delay between requests.

On 429: exponential backoff (e.g., 1 min → 2 min), then resume single-file.

---

## Failure Handling

Missing PV/depth → set fields to null/blank; still emit the row (don’t crash).

Network/5xx → retry up to 2× with backoff; if it still fails, mark the row as unavailable and continue.

MultiPV quirks → parse defensively; if you asked for 3 and got 5, keep the first 3.

---

## References
```
Lichess API docs https://lichess.org/api 

Cloud Eval endpoint (official docs):
https://lichess.org/api#operation/apiCloudEval 
lichess.org


MultiPV behavior (returns more lines than requested):
https://lichess.org/forum/lichess-feedback/cloud-eval-api-returns-more-variations-than-required 
lichess.org

FEN / en passant correctness discussion (EP field causing failures):
https://lichess.org/forum/lichess-feedback/wrong-fen-en-passant-square-ignored 
lichess.org

General API tips & rate-limit etiquette:
https://lichess.org/page/api-tips
```
