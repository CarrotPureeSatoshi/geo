---
description: Quickly score a short passage (a paragraph, a section, a draft snippet) for AI-search citability without producing a full audit report. Use when iterating on a single block of text. Trigger when the user asks "is this paragraph good for AI search", "score this snippet", "quick GEO check on this passage", or pastes a short block (< 500 words) and asks how it'd land with LLMs.
---

# Citability Score

A lightweight, fast scoring skill for short passages — paragraphs, sections, draft snippets, headlines. Designed for iteration: the user is rewriting and wants quick feedback in seconds, not a full audit.

## Inputs

A short passage, typically 50–500 words. Pasted directly or quoted from a file.

If the input is > 500 words, recommend `geo-audit` instead and stop.

## What to score (compressed rubric)

Score each from 0 to 5. Total citability = sum × 5 (so 0–100).

| Dimension | What to look for |
| --------- | ---------------- |
| **Extractable** | Can an LLM lift this passage as a self-contained answer? Does it stand on its own? |
| **Specific** | Concrete numbers, names, dates — or vague hand-waving? |
| **Sourced or honest** | Claims backed by sources, OR clearly framed as opinion. No unsupported "studies show". |
| **Quotable** | At least one crisp sentence an AI could cite verbatim and have it still make sense. |

## Output format

Keep it tight. No long report.

```
**Citability: XX / 100**

✓ <one specific strength, ≤ 12 words>
✗ <one specific weakness, ≤ 12 words>
✗ <one specific weakness, ≤ 12 words>

Quick fix: <one concrete change the author can apply right now>
```

That's it. No tables, no headers, no preamble.

## Rules

- Maximum 6 lines of output. The user is iterating — they don't want to read a wall.
- The "Quick fix" must be applicable in under 60 seconds (rephrase a sentence, add a number, split a paragraph). Not "do more research".
- If the score is ≥ 85, just say so and skip the weaknesses: `**Citability: 88 / 100** — solid as-is.`
- If the input is too short to score (< 30 words), say so and ask for more context.
- Never rewrite the passage here. Quick fix = a one-line instruction, not a rewrite.
