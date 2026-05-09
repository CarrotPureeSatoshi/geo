---
title: "How to add an llms.txt to a Next.js site in 5 minutes"
date: 2026-04-22
author: Example Author
---

# How to add an llms.txt to a Next.js site in 5 minutes

**TL;DR:** `llms.txt` is a plain-Markdown file at the root of your domain that tells AI crawlers which pages on your site are worth reading. As of April 2026, ~12,000 sites have adopted it (per [llmstxt.org](https://llmstxt.org/)). This post shows how to add one to a Next.js App Router project in three steps.

## What is llms.txt?

`llms.txt` is a proposed standard published in September 2024 by Jeremy Howard. It lives at `/llms.txt` on your domain (like `robots.txt`) and is written in Markdown. It contains:

1. An H1 with your site name
2. A blockquote summary (1–2 sentences)
3. H2 sections grouping links to your most important pages

Unlike `robots.txt`, it is not access control — it is curation. The goal is to point AI assistants at the pages that best represent your site.

## Step 1 — Create the file

In a Next.js App Router project, add a route handler at `app/llms.txt/route.ts`:

```typescript
export async function GET() {
  const body = `# Acme Docs

> Acme is a developer-tools company. These docs cover our CLI, SDK, and REST API.

## Getting started
- [Install the CLI](https://acme.dev/docs/install)
- [Quickstart tutorial](https://acme.dev/docs/quickstart)

## API reference
- [REST API](https://acme.dev/docs/api/rest)
- [SDK reference (TypeScript)](https://acme.dev/docs/api/sdk-ts)
`;

  return new Response(body, {
    headers: { 'Content-Type': 'text/markdown; charset=utf-8' },
  });
}
```

## Step 2 — Verify in dev

Run `next dev` and open `http://localhost:3000/llms.txt`. You should see the Markdown source as plain text.

## Step 3 — Deploy and verify with curl

After deployment:

```bash
curl https://your-domain.com/llms.txt
```

If you see your Markdown returned with `Content-Type: text/markdown`, you're done.

## Common mistakes

- **Returning HTML instead of Markdown.** Make sure the `Content-Type` header is `text/markdown`, not `text/html`.
- **Listing every page.** `llms.txt` is curation, not a sitemap. Pick 5–15 of your most important pages.
- **Forgetting to update it.** When you launch a major new section, add it. Stale `llms.txt` is worse than none.

## FAQ

### Do AI crawlers actually read llms.txt?

Adoption is partial. Anthropic, OpenAI, and Perplexity have all expressed interest publicly, but as of May 2026 none of them officially document using it as a primary signal. It's a low-cost bet for high upside.

### Does llms.txt replace robots.txt or sitemap.xml?

No. `robots.txt` controls access, `sitemap.xml` lists all crawlable URLs, `llms.txt` curates the most important content. Use all three.

### What if my site is not in English?

Write the file in your primary language. Multilingual sites can include sections per language with H2 headings.

## Next steps

If you found this useful, the natural follow-up is to verify your `robots.txt` doesn't block AI bots — see [Are AI crawlers reading your site?](/posts/ai-crawler-robots-txt).
