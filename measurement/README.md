[README.md](https://github.com/user-attachments/files/30893655/README.md)
# KORP Persona Coherence — Evidence Data

Redacted run logs from KORP persona coherence testing. All conversation text, persona DNA, scenario details, prompt templates, and API credentials have been stripped. Only quantitative scores and metadata remain.

## What this proves

A single AI persona maintained coherence (drift score 4.83-4.97/5) across:
- 60 conversational turns against Grok 4.5
- 20 turns against GPT-4O
- 20 turns against LLaMA-3.1-70B-Instruct

No character breaks. No prompt leakage. No collapse events.

## Files

| File | Description |
|------|-------------|
| `korp_run_20260809T165905Z.jsonl` | 20 turns, Grok 4.5 (baseline, pre length-bias adjustment) |
| `korp_run_20260809T171227Z.jsonl` | 60 turns, Grok 4.5 (main run) |
| `korp_run_20260810T063202Z.jsonl` | 20 turns, GPT-4O |
| `korp_run_20260810T063954Z.jsonl` | 20 turns, LLaMA-3.1-70B |
| `korp_run_20260810T064933Z.jsonl` | 21 turns, LLaMA-3.1-70B (aborted at 21/80) |
| `personagym_reeval_final_20260810T031334Z.jsonl` | PersonaGym evaluation (GPT-4O + LLaMA ensemble judges) |
| `redaction_manifest.json` | Redaction policy and file checksums |

## Data format

Each JSONL entry contains scores and metadata only:

```json
{
  "kind": "conversation_turn",
  "turn": 1,
  "phase": "early",
  "drift_score": 5.0,
  "scores": {
    "expected_action": 5,
    "linguistic_habits": 5,
    "persona_consistency": 5,
    "toxicity_control": 5,
    "action_justification": 5
  },
  "persona_length_bucket": "16-30",
  "target_model": "grok-4.5",
  "logged_at": "2026-08-09T17:12:52.720629+00:00"
}
```

## Redaction policy

- All conversation text removed (persona and target)
- Word counts bucketed (1, 2-5, 6-15, 16-30, 31-60, 61+) to prevent reconstruction
- Probe hashes salted with ephemeral random salt
- No API keys, prompt templates, or scenario details included

## Length bias finding

The PersonaGym external evaluation (standard rubrics, GPT-4O + LLaMA judges) scored this run at 3.62/5 overall. Short replies (<=30 chars, 43% of turns) scored 2.58/5 while longer replies scored 4.41/5 — a gap of +1.83 on a 5-point scale. Toxicity Control (the only dimension measuring absence rather than presence) showed no length bias (4.94 vs 4.90).

## Contact

KORP Systems / Mio Oscarsson
