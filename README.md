# Day 5 — Meeting Intelligence Assistant (MIA)

Working prototype: **[open the app](https://claude.ai/artifact/Lpe7s9QWWPkXwnMsiGdjQp)**

## What this is

MIA turns pasted meeting notes into structured decisions, action items,
risks, open questions, and ambiguities — conservatively, with evidence for
every claim, per the SRS's output contract (Section 8).

## How to use it

1. Open the app link above.
2. Click one of the five sample-scenario chips (or paste your own meeting
   text).
3. Choose a prompt version — **v2 · structured** is the real product
   prompt; **v1 · basic** exists only for comparison.
4. Click **Analyze**. The app calls Claude directly using your own Claude
   access (nothing is sent until you click).
5. Review the result. Every field (summary, decision text, task, owner,
   deadline, ambiguity) is editable in place — click it and type.
6. Export **Download JSON** or **Download CSV (action items)**.

## How it's built

- Single self-contained HTML/CSS/JS page — no backend, no build step, no
  API key of my own to manage. The model call goes through the artifact
  platform's built-in `sample` capability, which is exactly the "isolate
  provider-specific code behind a small function" requirement in SRS
  Section 11 — the whole model call is one function (`sampleAPI.json(...)`),
  swappable without touching the UI.
- The prompt is stored as a plain JS string, separate from the UI code
  (SRS Section 11 / Section 16's "the prompt itself is saved as a reusable
  asset").
- Error states covered: empty input (blocked before any call), no model
  access, rate limiting, unparseable JSON (raw reply shown for debugging),
  and a short-input warning.

## Package contents

| File | Maps to submission requirement |
|---|---|
| `prompt_v1_basic.md` | Prompt v1 |
| `prompt_v2_structured.md` | Prompt v2 + eight-layer breakdown |
| `sample_outputs_v2.md` | Sample output for all five meetings |
| `failure_analysis.md` | Evaluation notes showing v1 → v2 changes |
| This README | Setup/how to run + limitations |

## Limitations

- No live transcription, calendar/Slack integration, auth, or persistent
  storage — matches the SRS's explicit non-goals (Section 4).
- The `sample` capability caps a single call at 64 KiB of input text, so a
  very long transcript would need to be chunked — not implemented, since
  none of the five test scenarios approach that size.
- Editing a field updates the in-memory result and the exported files, but
  edits aren't re-validated against the schema — a manually-typed priority
  value, for instance, won't be constrained to high/medium/low.
- v1's outputs weren't captured by me in advance (see
  `failure_analysis.md` for why) — capture your own before submitting.

## Still to do before submitting

- Run both prompt versions against scenario 5 in the live app and
  screenshot both (the clearest before/after evidence).
- Do the same for at least one more scenario.
- Write your own one-paragraph engineering note on what you observed,
  using `failure_analysis.md` as a starting structure, not a script to
  copy.
