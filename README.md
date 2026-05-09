# GEO — Generative Engine Optimization for Claude Code

A Claude Code plugin that helps developers and technical writers make their docs, blog posts, and landing pages more **citable by AI search engines** — ChatGPT, Claude, Perplexity, Google AI Overviews, Bing Copilot.

Think of it as **ESLint for LLM discoverability**: audit your content, score it, and get specific, prioritized fixes.

## Why this exists

AI-powered search now sits in front of traditional search for a fast-growing share of queries. The pages that get cited are not always the pages that rank first on Google — they're the pages that are *extractable*: well-structured, factually grounded, schema-marked, and self-contained at the section level.

This plugin gives you the tools to evaluate and improve your own content for that surface, without sending it to a third-party SaaS.

## What's in the box

Six skills and one coordinator agent:

| Component | What it does |
| --------- | ------------ |
| `/geo:geo-audit` | Full citability audit on a file or URL. Scored report (0–100) with prioritized fixes. |
| `/geo:geo-rewrite` | Rewrite content for citability while preserving the author's voice. |
| `/geo:citability-score` | Quick score on a short passage. For iterating fast. |
| `/geo:schema-markup` | Generate valid JSON-LD (Article, FAQ, HowTo, Product) ready to paste. |
| `/geo:llm-readiness-check` | Technical/infra audit: AI crawler rules, `llms.txt`, sitemap, render-without-JS. |
| `/geo:competitive-citations` | Compare your page to 2–3 competitors on the same topic. |
| `geo-auditor` agent | Coordinator for full-site audits. Orchestrates the skills above and synthesizes one report. |

## Quick start

1. Install the plugin:
   ```
   /plugin marketplace add CarrotPureeSatoshi/geo
   /plugin install geo@geo
   ```

2. Audit a single file:
   ```
   /geo:geo-audit path/to/your-blog-post.md
   ```

3. Get a quick score on a paragraph you're drafting:
   ```
   /geo:citability-score
   ```
   …then paste the paragraph.

4. Run the full coordinator on a docs folder:
   ```
   Use the geo-auditor agent on docs/
   ```

## Examples

### Before / after a `geo-rewrite`

**Before** (citability ~ 42/100):

> Many businesses today are leveraging AI to drive results. In an ever-changing landscape, it's important to think about how your content will be discovered by next-generation search experiences.

**After**:

> **TL;DR:** AI-search citations grew 4× year-over-year in 2025 (Search Engine Land, Jan 2026). To get cited, your content needs three things: scannable structure, specific claims, and a quotable lede. This post walks through each.
>
> Most blog posts written before 2024 fail one or more of these. Here's how to spot the gaps and fix them.

### Schema markup for a how-to

Input: a tutorial markdown file with 5 numbered steps.

Output: a `<script type="application/ld+json">` block with `@type: HowTo`, `totalTime`, and a `step` array — ready to paste in your `<head>`.

### Site-wide audit

```
> Use the geo-auditor agent. Audit https://blog.example.com — sample the 5 most recent posts.

→ Overall readiness: Partial
→ Average citability: 61/100 (range 48–79)
→ Top 5 fixes ranked by impact × effort:
   1. Add llms.txt (missing) — High impact, S effort
   2. Add TL;DR to all posts (3/5 missing) — High impact, S effort
   3. Replace vague claims with numbers in posts A and C — Medium, M
   ...
```

## What this plugin does NOT do

- **No external SaaS calls.** The plugin doesn't send your content to a third-party AI-SEO tracking service. All analysis is local — Claude reads your files and reasons about them.
- **No live LLM citation tracking.** It doesn't tell you "ChatGPT cited you 14 times this week" — that's a separate problem (and a separate kind of tool). What it does is make your content *more likely* to be cited.
- **No traffic data, no rankings.** No DataForSEO, no Search Console integration. Out of scope.
- **No automatic file modification.** Recommendations only — you decide what to apply.
- **No content generation from scratch.** It rewrites and improves. It won't write a blog post you haven't drafted.

## Privacy & security

- **All analysis is local to your Claude Code session.** Your content is read by Claude, not sent to any external GEO-analysis API.
- **`llm-readiness-check` and `competitive-citations` use `WebFetch`** on URLs *you* provide — they don't crawl beyond what you ask for.
- **No telemetry.** The plugin doesn't phone home.
- **MIT licensed** — fork it, audit it, modify it.

## Repository layout

```
.
├── .claude-plugin/
│   └── marketplace.json     ← marketplace manifest (this is what /plugin marketplace add reads)
├── plugin/                  ← the plugin itself
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── skills/
│   └── agents/
├── tests/examples/          ← good/bad post fixtures
├── README.md
├── LICENSE
└── CHANGELOG.md
```

## How citability is scored

The scoring rubric covers ten dimensions:

1. Extractability (structure, hierarchy, scannable)
2. Self-contained answers (each section stands alone)
3. Factuality and sources
4. Freshness signals
5. Entity clarity
6. Schema-friendliness
7. Voice and originality
8. Technical accessibility
9. Internal coherence
10. Linkability

Each is scored 0–10, then averaged ×10 to give a 0–100 citability score. The full rubric is documented in `skills/geo-audit/SKILL.md`.

## When to use which skill

- **Drafting a single paragraph?** → `citability-score`
- **Just finished a post?** → `geo-audit`, then `geo-rewrite`
- **Adding a new section to your site?** → `schema-markup`
- **New site, never audited?** → `llm-readiness-check` first, then `geo-auditor` agent
- **Trying to figure out why a competitor outranks you in AI answers?** → `competitive-citations`

## Roadmap

- v0.2 — `geo-faq-generator` skill (extract implicit questions from a post into an FAQ block)
- v0.2 — `update-old-content` skill (re-audit + targeted refresh suggestions for stale posts)
- v0.3 — Optional MCP server for cite-tracking integrations (Profound, Peec AI, Otterly) — opt-in, with explicit user-supplied API keys

## Author

Built by **Kevin Gibaud** ([Swoft](https://swoft.ai)) — kevin.gibaud@devapps4biz.com

## License

MIT — see [LICENSE](LICENSE).

## Contributing

Issues and PRs welcome at https://github.com/CarrotPureeSatoshi/geo. For substantive changes, please open an issue first to discuss the approach.
