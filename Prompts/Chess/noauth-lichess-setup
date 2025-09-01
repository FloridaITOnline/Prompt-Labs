# Lichess Cloud Eval — No Auth (Free)

**Step 1 of 2** → produces normalized analysis data that your `chessanalysis.md` (Step 2) will format. This specific prompt does not require any Tokens or Auth.

---

## What this is
A guide + protocol for using Lichess’s free, unauthenticated **Cloud Evaluation** endpoint to evaluate positions derived from a pasted PGN.

## What this isn’t
- Does not require API keys  
- Does not require login  
- Does not require servers or local engines  

---

## Endpoint (no login needed)

```http
GET https://lichess.org/api/cloud-eval?fen=<URL-ENCODED-FEN>[&multiPv=N]

Minimal Flow (PGN → FENs → Cloud Eval → JSON hand-off)

User pastes PGN into your interaction window.

Parse PGN and reconstruct the position after each full move (or every ply if you want finer granularity).

For each position, build a legal FEN (standard chess, not variants) and call Cloud Eval.

Normalize the response into your Step-1 JSON (see schema below).

Pass the JSON to chessanalysis.md (Step 2) for CSV/summary formatting.

💡 Tip: evaluating every full move roughly halves requests while staying useful.

## Request Samples

GET https://lichess.org/api/cloud-eval?fen=rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR%20b%20KQkq%20-%200%201
GET https://lichess.org/api/cloud-eval?fen=<FEN>&multiPv=3

## Example Response

{
  "fen": "<same fen>",
  "depth": 20,
  "pvs": [
    { "moves": ["e2e4","c7c5", "..."], "cp": 23 },
    { "moves": ["d2d4","d7d5", "..."], "cp": 18 }
  ]
}

## Step-1 Output (normalized JSON) → Input to chessanalysis.md
White POV Normalization Rule

If the returned score is for the side-to-move:

White to move: whitePOV = cp (or mate as +N)

Black to move: whitePOV = -cp (or mate as -N)

Recommended Step-1 JSON Row
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

> Note :FEN Correctness (important) 
>> Use standard chess FEN (not variant FENs). Some variants (e.g., crazyhouse) won’t work without conversion.
>> Ensure en passant, castling rights, and move counters are valid. Incorrect EP fields are a known cause of failures.
