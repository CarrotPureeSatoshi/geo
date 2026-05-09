---
description: Compare a piece of content against 2–3 competitors on the same topic to identify why an LLM might cite one over the other. Inputs are local files or URLs the user provides. No paid API. Trigger when the user asks "why does X get cited and not me", "compare my post to these competitors for AI search", "which of these would Perplexity cite", or shares their content + 2-3 competitor links.
---

# Competitive Citations Analysis

Side-by-side comparison of multiple pieces of content on the same topic, evaluated through the lens of AI search citation likelihood. No external paid API — the user provides the inputs.

## Inputs

The user provides:
1. **Their content** — file path or URL
2. **2–3 competitors** — file paths or URLs on the same or near-identical topic
3. Optional: the **target query** they want to be cited for ("how to deploy Next.js to Vercel")

If they only provide their content, ask which competitors to compare against. Do not invent competitors.

## Method

For each piece of content:

1. **Fetch / read** the content.
2. **Score each piece on the same compressed rubric** (reusing `geo-audit` dimensions, but lighter — score 0–10 per dimension, 5 dimensions):
   - Extractability
   - Specificity (numbers, names, dates)
   - Sources & factuality
   - Entity clarity
   - Unique angle / first-hand details

3. **Pull the same 1–2 representative passages** from each piece (the section that most directly answers the target query) so they can be compared side-by-side.

4. **Identify what each piece does best** and **what each piece does worst** for AI citation.

## Output format

```
# Competitive Citations — <target query>

## Summary
| Source | Citability | Strongest dimension | Weakest dimension |
| ------ | ---------- | ------------------- | ----------------- |
| **You** (your-file.md) | XX/50 | <dim> | <dim> |
| Competitor A | XX/50 | <dim> | <dim> |
| Competitor B | XX/50 | <dim> | <dim> |

## What competitors do better than you
1. **<Competitor A>** — <specific thing>. Example passage:
   > "<short quote>"
   Why it wins for AI search: <one line>

2. ...

## What you already do better
1. <thing> — <one line>

## Where you can leapfrog (prioritized)
1. <action> — <impact for the target query>
2. ...

## Quotable passages comparison
For the query "<target>", the LLM's likely picks:
- **Most likely cited**: <Competitor X> — passage that's specific + sourced + standalone
- **Yours, after fix**: <what would change> — passage you could write to win
```

## Rules

- **Pull real quotes**, not paraphrases. Cite line numbers if reading from files.
- **Stay even-handed**. Don't tell the user theirs is best to flatter them. If a competitor is genuinely stronger, say so and explain why.
- **Focus on what's actionable.** "Their tone is friendlier" is not actionable. "Their FAQ section answers 6 questions yours skips" is.
- **Do not crawl deeply.** Fetch only the URLs the user provides.
- **No paid APIs.** No SaaS lookups for traffic, rankings, or citations — those are out of scope. This skill works entirely from the content the user provides.
- If competitor URLs are paywalled or blocked, say so and ask for plain-text excerpts instead.
