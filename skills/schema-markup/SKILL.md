---
description: Generate JSON-LD structured data (schema.org) for a piece of content so AI search engines can extract entities, FAQs, how-tos, articles or products reliably. Outputs ready-to-paste `<script type="application/ld+json">` blocks. Trigger when the user asks "generate schema markup", "add JSON-LD", "make this FAQ structured", "schema for this how-to", or wants to make a page more machine-readable.
---

# Schema Markup Generator

Generate valid schema.org JSON-LD for a piece of content, so AI search engines (and traditional search engines) can extract its structure cleanly.

## Inputs

- A file path, URL, or pasted content
- Optionally, the schema type the user wants (Article, FAQPage, HowTo, Product, Organization, BreadcrumbList, SoftwareApplication, etc.)

If the type is not specified, infer it from the content:
- Numbered steps with actions → `HowTo`
- Q&A pairs → `FAQPage`
- Long-form opinion/explanation → `Article` or `BlogPosting`
- Product description with price/spec → `Product`
- Comparison of options → multiple `Product` or `SoftwareApplication` items in an `ItemList`

If multiple types apply, generate the primary one and mention the alternatives.

## What to generate

A single `<script type="application/ld+json">` block (or two, if the page genuinely has two distinct types — e.g. an Article that contains a FAQ section).

### Required fields per type

**Article / BlogPosting**
- `@context`, `@type`, `headline`, `author`, `datePublished`, `dateModified`, `description`, `image` (if available), `mainEntityOfPage`

**FAQPage**
- `@context`, `@type: FAQPage`, `mainEntity` (array of `Question` with nested `acceptedAnswer`)
- Each `Question.name` is the question, each `acceptedAnswer.text` is the full answer

**HowTo**
- `@context`, `@type: HowTo`, `name`, `description`, `totalTime` (ISO 8601), `step` (array of `HowToStep` with `name` and `text`)

**Product / SoftwareApplication**
- `@context`, `@type`, `name`, `description`, `offers` (with `price`, `priceCurrency`), `aggregateRating` only if real ratings exist

## Rules — strict

1. **Never invent values.** If the publication date isn't in the source, ask for it or use a `<!-- TODO: datePublished -->` placeholder. Same for author, image URLs, prices.
2. **Match content exactly.** FAQ questions in the JSON-LD must match the H2/H3 questions in the visible content verbatim. Answers must match what the page actually says — schema and content disagreement is an SEO penalty.
3. **Use ISO 8601 dates** (`2026-05-09`, `PT15M`).
4. **Do not include `aggregateRating`, `reviewCount`, or `Review`** unless the source content actually shows those values. Fake ratings get pages penalized.
5. **Escape correctly** — quotes inside string values must be escaped, no trailing commas (JSON, not JS).
6. **Validate mentally** — output should pass [Google's Rich Results Test](https://search.google.com/test/rich-results) without errors. If you're not sure, flag what to verify.

## Output format

```json
<!-- Paste this into your <head> -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "...",
  ...
}
</script>
```

After the block, add a short list:

```
## Notes
- Type chosen: <Type> (because <reason>)
- Fields filled from content: headline, datePublished, ...
- Fields needing your input: <list of TODOs>
- Validation: paste into https://search.google.com/test/rich-results to verify
```

If you generate two blocks (e.g. Article + FAQPage), separate them clearly.

## What this skill does NOT do

- Does not modify the source HTML or markdown — it only outputs the JSON-LD block. The user inserts it themselves.
- Does not generate schemas for content the page doesn't actually contain (no fake reviews, no fabricated ratings, no inflated step counts).
- Does not call external APIs.
