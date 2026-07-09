# KB Article Template & Inventory Conventions

## Article structure

```markdown
# {Task-shaped title}            <!-- "How to export your data", "Why your invoice shows two line items" -->

{1-2 sentence answer-first summary — the reader should be able to stop here.}

## Steps                          <!-- for how-to articles -->
1. {Numbered, one action per step, name UI elements in **bold**}
2. …

## What to expect                 <!-- optional: result, timing, confirmation email, etc. -->

## Troubleshooting / edge cases   <!-- optional: the 2-3 variants that actually appear in tickets -->
- **{Symptom}** — {fix or explanation}

## Related articles
- [{title}]({slug-or-url})
```

Rules:
- **Title = the customer's question**, phrased the way they'd search it ("How to…", "Why does…", "Can I…"). Never internal jargon.
- **Answer first.** The summary resolves the common case before any steps.
- One article = one task. Two tasks = two articles with cross-links.
- Screenshots: insert `[Screenshot: {what to capture}]` placeholders — the user adds real images.
- No customer specifics ever: no names, emails, account IDs, or verbatim private messages.
- Length target: 150-400 words. Longer → split.

## FAQ entry structure

```markdown
**Q: {Question as customers ask it}?**
A: {2-5 sentences, answer-first. Link the full article if one exists.}
```

Group FAQ entries by topic tag (billing, account, how-to…). An answer past ~5 sentences is a promotion candidate to a full article.

## Article inventory

One row per article, mirrored to `KB.inventory_path` if set:

```markdown
| Title | Slug | Topics | Status | Last updated |
|---|---|---|---|---|
| How to export your data | export-data | how-to, account | published | 2026-07-09 |
```

Statuses: `draft` → `published` → `stale` (references changed behavior; needs review).

Memory mirror (one entry per article): `sup: {PRODUCT} kb article: {slug} — {title} — status: {status} — covers: {topics}`.

## Deflection-first ordering

When ranking "what to write next":

```
deflection score = frequency × self-serve likelihood
```

- **Frequency** — times the question appeared in the analyzed period (state the period).
- **Self-serve likelihood** — high: how-to, settings, billing mechanics, integrations. Medium: account/data requests (article helps, human still confirms). Low: bugs, refund decisions, anything requiring judgment — don't write articles hoping to deflect those; fix or route them instead.

Present the queue as: `# | Proposed article | Frequency (n, period) | Likelihood | Score | Why`.
