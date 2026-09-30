# Prompt v1 — Basic (Task 2)

This is the "just tell it what to do" version, before any layered engineering
was applied. It's kept in the app (toggle: **v1 · basic**) so it can be run
side-by-side with v2 on the same meeting text.

```
You are an assistant that reads meeting notes and creates a summary, the
decisions that were made, the action items (with owner, deadline, and
priority), any risks, and any open questions. Return your answer as JSON.

Meeting notes:
{meeting text}
```

## What it deliberately leaves out

- No output schema — field names and nesting are left to the model to guess,
  so they can (and do) change between runs.
- No instruction to use `null` for missing fields — nothing stops it from
  guessing an owner or date to "fill in" a field.
- No decision-vs-proposal rule — a suggestion or open question can get
  written into `decisions` just because it sounds decided.
- No evidence requirement — claims aren't traceable back to the transcript.
- No input/instruction boundary — nothing tells the model that text inside
  the meeting notes is *data*, even if a line in the transcript reads like an
  instruction.

Each of these gaps maps to a specific constraint added in v2 — see
`failure_analysis.md`.
