---
description: Rewrite a piece of content to maximize citability by AI search engines while preserving the author's voice. Restructures into TL;DR + scannable sections, tightens claims, surfaces sources, and prepares the text for schema markup. Trigger when the user asks "rewrite this for AI search", "make this GEO-ready", "improve this post for LLMs", or after a `geo-audit` run when they want the fixes applied.
---

# GEO Rewrite

You rewrite written content (blog posts, docs, landing pages) so that AI search engines are more likely to cite it — without flattening the author's voice into generic AI prose.

## Inputs

- A file path or pasted content
- Optionally, a prior `geo-audit` report to act on
- Optionally, the author's voice notes ("keep it punchy", "stay technical", "first person OK")

If voice notes are missing, infer voice from the input: cadence, sentence length, vocabulary, point of view, use of "I" / "we" / "you".

## Transformations to apply

1. **Add a 2–3 sentence TL;DR** under the H1, before any preamble. State the core claim and who the post is for.
2. **Promote the buried lede.** If the strongest insight is in paragraph 4, move it to paragraph 1.
3. **Break walls of text.** Any paragraph > 4 sentences gets split or converted to a list.
4. **Add scannable structure.** H2 per major question/section. H3 only when needed. Stable anchor-friendly headings (no emojis in headings).
5. **Tighten claims.** Replace "many", "often", "usually" with a number, a date, or a named example. If the original truly didn't have evidence, mark it as opinion ("In our experience…") rather than disguising it as fact.
6. **Name entities fully on first mention.** "Claude" → "Anthropic's Claude" (first mention only).
7. **Surface sources inline.** Convert "studies show" into "[Source name (2025)](url)".
8. **Add a quotable sentence per section.** One crisp claim per H2 that an LLM could extract verbatim and that still makes sense out of context.
9. **Insert a FAQ block at the end** if the topic is question-shaped. 3–6 questions a real user would search, one short answer each.
10. **Fix the conclusion.** Replace "In conclusion" / "To wrap up" with a concrete next action.

## What NOT to do

- **Do not flatten voice.** If the author uses contractions, dry humor, or first person, keep it. Don't sand off personality.
- **Do not invent facts, sources, statistics, or quotes.** If a claim needs evidence and there isn't any, mark it as opinion or flag it for the author to fill in: `<!-- TODO: add source -->`.
- **Do not pad.** If the original is tight, the rewrite should be roughly the same length or shorter.
- **Do not add emojis** unless the original used them.
- **Do not change the core argument.** GEO rewrite is structural and clarity-focused, not editorial.
- **Do not insert generic boilerplate** ("In today's fast-paced digital world…"). Cut every sentence that could appear in any other article on any other topic.

## Output format

Return the rewritten content as a single Markdown block, ready to copy back into the source file.

After the rewrite, append a short changelog:

```
---

## What changed
- Added TL;DR (3 sentences)
- Moved key insight from paragraph 4 to paragraph 1
- Split 6 long paragraphs into lists/shorter blocks
- Replaced 4 vague claims with numbers/named examples
- Inserted FAQ block (5 Q&A) at end
- Flagged 2 claims for sourcing: see `<!-- TODO -->` markers
```

## Rules

- Output the rewrite first, the changelog second. Nothing else.
- Preserve all code blocks, tables, and image references unchanged unless they need restructuring.
- If the original is already GEO-strong (score would be > 85), say so and return only minor edits — don't manufacture changes.
- Never send the user's content to an external service.
