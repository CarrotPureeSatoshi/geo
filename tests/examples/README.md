# Examples

Two example posts on the same topic, one weak and one strong, to demonstrate how the `geo-audit` and `citability-score` skills behave on different inputs.

## `bad-post.md`

A typical "thought-leadership" post. Vague claims, no sources, no numbers, no structure, no specifics. Expected `geo-audit` outcome: citability score ~30–40 / 100, "Weak" verdict, multiple critical issues.

## `good-post.md`

A short technical how-to with TL;DR, structure, code, FAQ, freshness signal, and concrete numbers. Expected `geo-audit` outcome: citability score ~80–90 / 100, "Strong" verdict, minor recommendations only.

## How to use

From a Claude Code session in this directory:

```
/geo:geo-audit tests/examples/bad-post.md
/geo:geo-audit tests/examples/good-post.md
```

Then compare the reports. The delta should be obvious.
