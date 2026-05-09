---
description: Technical audit of a website or repo for LLM/AI-crawler readiness — checks robots.txt rules for AI bots (GPTBot, ClaudeBot, PerplexityBot, etc.), presence and validity of llms.txt, sitemap availability, response headers, and basic accessibility signals that affect AI extraction. Trigger when the user asks "is my site ready for AI crawlers", "do LLMs see my site", "check robots.txt for AI bots", "do I have an llms.txt", or shares a domain/URL and asks about AI search infrastructure.
---

# LLM Readiness Check

Technical, infrastructure-level audit. Distinct from `geo-audit` (which is content-level). This skill verifies that AI crawlers can actually reach and parse the content in the first place.

## Inputs

- A domain or URL (e.g. `https://example.com`)
- Optionally, a path to a local repo if the user wants to check `robots.txt`, `llms.txt`, `sitemap.xml` files in source rather than over the network

## What to check

### 1. `robots.txt` — AI crawler rules

Fetch `<domain>/robots.txt`. Identify rules for these user-agents:

| User-agent | Operator | Common purpose |
| ---------- | -------- | -------------- |
| `GPTBot` | OpenAI | Training + ChatGPT browse |
| `ChatGPT-User` | OpenAI | Live ChatGPT browse on user request |
| `OAI-SearchBot` | OpenAI | ChatGPT Search index |
| `ClaudeBot` | Anthropic | Training |
| `Claude-Web` / `claude-web` | Anthropic | Live retrieval |
| `anthropic-ai` | Anthropic | Older identifier |
| `PerplexityBot` | Perplexity | Index |
| `Perplexity-User` | Perplexity | Live retrieval on user request |
| `Google-Extended` | Google | Gemini training (separate from Googlebot) |
| `Bingbot` | Microsoft | Powers Bing Copilot |
| `CCBot` | Common Crawl | Used by many LLMs as training data |
| `Bytespider` | ByteDance | TikTok / Doubao |
| `Applebot-Extended` | Apple | Apple Intelligence training |
| `Meta-ExternalAgent` / `FacebookBot` | Meta | Llama training |
| `Amazonbot` | Amazon | Alexa / Rufus |
| `DuckAssistBot` | DuckDuckGo | DuckAssist |
| `cohere-ai` | Cohere | Training |
| `MistralAI-User` | Mistral | Live retrieval |
| `YouBot` | You.com | You.com search |

For each, report: **Allowed**, **Disallowed**, or **Not specified** (which usually means allowed by default rules).

Flag conflicts: e.g. `Disallow: /` for `*` but no explicit `Allow` for AI bots = AI bots are blocked.

### 2. `llms.txt`

Fetch `<domain>/llms.txt`. Check:
- Does it exist?
- Is it valid Markdown with the [proposed format](https://llmstxt.org/) (H1 site name, blockquote summary, sections with H2 headings and links)?
- Does it list the most important pages a curious LLM would want?

If absent, recommend creating one (small effort, growing adoption signal).

### 3. `sitemap.xml`

Fetch `<domain>/sitemap.xml` (and check `Sitemap:` directive in `robots.txt`).
- Does it exist?
- Does it list at least the main content pages?
- Are `<lastmod>` dates fresh?

### 4. Response headers (sample one or two pages)

For a representative page, check:
- `Content-Type` — `text/html; charset=utf-8` ideally
- `X-Robots-Tag` — any `noai`, `noimageai`, `noindex`?
- `Cache-Control` — not a hard requirement but flag if pages are uncacheable
- TLS — HTTPS only? mixed content? expired cert?

### 5. Render-without-JS check

Fetch the raw HTML of one content page. Does the actual article text appear in the HTML, or is the body empty until JavaScript runs?
- AI crawlers vary in JS execution. Pure-CSR (client-side rendered) sites often lose 50–80% of citations.
- If empty: recommend SSR/SSG (Next.js, Astro, Eleventy) or pre-rendering.

### 6. Other signals

- `<title>` and `<meta name="description">` present and non-generic
- Canonical URL set
- Open Graph + Twitter Card tags (helps disambiguation)
- Structured data presence (`<script type="application/ld+json">`) — if missing, suggest the `schema-markup` skill

## Output format

```
# LLM Readiness Check — <domain>

**Overall: Ready / Partial / Blocked / Critical**

## AI Crawler Access (robots.txt)
| Bot | Status | Note |
| --- | ------ | ---- |
| GPTBot | Allowed | |
| ClaudeBot | Disallowed | Blocked by `Disallow: /` for `*` with no override |
| ...

## llms.txt
- Present: yes / no
- Valid format: yes / no
- Coverage: <summary>

## sitemap.xml
- Present: yes / no
- Pages listed: N
- Freshness: most recent lastmod = <date>

## Rendering
- Content visible without JS: yes / no
- Critical impact if no: <note>

## Recommendations (prioritized)
1. <action> — <impact>
2. ...
```

## Rules

- Use `WebFetch` only on the user-provided domain. Do not crawl deeply.
- Limit to 3–5 fetched URLs total: `/robots.txt`, `/llms.txt`, `/sitemap.xml`, and 1–2 sample content pages.
- If the domain blocks the fetch, say so — don't fabricate the content of those files.
- Recommend changes, do not apply them. The user owns their site config.
