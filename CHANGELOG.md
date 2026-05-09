# Changelog

All notable changes to the `geo` plugin are documented here. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] — 2026-05-09

Initial public release.

### Added
- Skill `geo-audit` — full citability audit on a file or URL with scored report (0–100)
- Skill `geo-rewrite` — rewrite content for citability while preserving author voice
- Skill `citability-score` — quick score for short passages, designed for iteration
- Skill `schema-markup` — generate valid JSON-LD (Article, FAQ, HowTo, Product) blocks
- Skill `llm-readiness-check` — technical audit of robots.txt, llms.txt, sitemap, render-without-JS
- Skill `competitive-citations` — side-by-side comparison of a page vs 2–3 competitors
- Agent `geo-auditor` — coordinator for full-site audits
- Test fixtures: good and bad markdown examples in `tests/examples/`
