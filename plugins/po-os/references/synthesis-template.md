# Synthesis Template — Clustering & Strength Rubric

Guidance for turning raw observations into named patterns with measured strength.

## Observation Data Model

Each raw signal becomes an **observation** with this shape:

```json
{
  "id": "obs_{nnn}",
  "source_type": "support_email | stripe_churn | competitor_review | forum | analytics | interview | nps_csat",
  "source_ref": "{URL, ticket ID, interview note ref}",
  "date_observed": "YYYY-MM-DD",
  "persona_fit": "{persona name or 'unknown'}",
  "market": "{country or 'any'}",
  "raw_excerpt": "{quote — no PII}",
  "theme_tags": ["{tag1}", "{tag2}"],
  "addressability": "yes | partial | no",
  "sentiment": "pain | frustration | request | confusion | delight"
}
```

Store observations (append-only) in `<DATA_DIR>/discovery-log.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`) so they accumulate across sessions.

## Clustering Method

Group observations into **patterns** when they share:

1. **Theme tags** (≥ 1 common tag)
2. **Addressability** (same type of product response)
3. **Persona fit** (same persona, or broadly applicable)
4. **Root-cause hypothesis** (same underlying issue, different surface)

A pattern gets a **name** (short, evocative, imperative): e.g., "cookie banner too technical", "no bulk DSAR export", "Italian policy reads machine-translated", "onboarding abandons at site-scan step".

## Strength Rubric

```
Strength = breadth × recency × triangulation
```

| Factor | Definition | Rubric |
|---|---|---|
| **Breadth** | Number of distinct sources reporting the pattern | 1-2 = weak, 3-5 = medium, 6+ = strong |
| **Recency** | How recent the signals are | Mostly last 30 days = 1.0 weight; last 90 days = 0.7; older = 0.4 |
| **Triangulation** | Number of distinct source **types** | 1 type = weak, 2 types = medium, 3+ types = strong |

**Final strength label:**

| Weak | Medium | Strong |
|---|---|---|
| 1 source or 1 source-type | 2-3 sources across 1-2 types | 3+ sources across 2+ types |

## Output Routing

| Strength | Recommended Output |
|---|---|
| Weak | Named hypothesis in memory; don't create backlog items unless it's a cheap test |
| Medium | Discovery Insight (pattern report) with 1-3 derived backlog items at P2-P3 |
| Strong | Backlog Items directly, P0-P1, with high-confidence acceptance criteria |

## Common Theme Tags (reusable taxonomy)

Use these consistently so cross-session search works:

### UX
- `ux-confusion` — user doesn't know what to do
- `ux-friction` — multiple steps when 1 would suffice
- `ux-hidden` — feature exists but not discoverable
- `ux-tone` — copy feels wrong (too technical, too legal, too salesy)

### Content / Legal
- `content-wrong` — generated text doesn't match user's situation
- `content-language` — not in user's language or poorly translated
- `content-outdated` — template doesn't match current law
- `content-local` — missing jurisdictional nuance

### Feature Gaps
- `gap-export` — can't get data out
- `gap-import` — can't get data in
- `gap-bulk` — no bulk operations
- `gap-automation` — manual step that should be automated
- `gap-integration` — missing 3rd-party connection
- `gap-role` — missing workflow for a specific role (DPO, lawyer, etc.)

### Performance / Reliability
- `perf-slow` — feature works but too slow
- `perf-broken` — feature broken
- `perf-limits` — hits usage limits

### Pricing
- `price-high` — perceived too expensive
- `price-unclear` — confusing plans
- `price-tier-fit` — features distributed across tiers in ways that frustrate

### Trust
- `trust-security` — concerns about data handling
- `trust-legal-credibility` — user unsure if output is "correct enough"
- `trust-local` — wants local brand / customer / lawyer backing

## Bias Flagging

Always annotate patterns with known biases:

- **Selection bias:** "Drawn from users who contacted support — silent majority not represented"
- **Recency bias:** "Heavy weight on last 7 days; may be a transient blip not a pattern"
- **Power-user bias:** "All 6 observations come from users in our top decile of usage"
- **Churner bias:** "All observations come from people who cancelled; happy users not in sample"
- **Anglophone bias:** "Forum mining was English-only; Italian/German/French signal not captured"

The PO reader should know which biases to adjust for.

## Clustering Workflow (suggested)

1. **Ingest** — read all enabled sources for the time window
2. **Tag** — apply theme tags to each observation (reuse taxonomy above; extend sparingly)
3. **Cluster** — group observations sharing theme + addressability + persona
4. **Name** — give each cluster a short imperative name
5. **Score** — compute strength per cluster
6. **Filter** — drop `addressability: no` clusters (or shunt them to a non-product list for ops)
7. **Derive** — produce Backlog Items or Discovery Insights per surviving cluster
8. **Persist** — append observations and pattern records to `discovery-log.json`
9. **Memory-store** — record strongest patterns in claude-mem with `po:` prefix

## Example Pattern

**Pattern:** `cookie-banner-copy-too-technical`
- **Theme tags:** `ux-confusion`, `content-wrong`, `ux-tone`
- **Persona fit:** SME Founder (non-lawyer)
- **Observations:**
  - Support ticket #318: "the cookie banner uses words I don't understand" (2026-03-12)
  - G2 review of a competitor (similar complaint): "too lawyerly for my customers" (2026-02-28)
  - r/SaaS post: "why do cookie banners talk like lawyers?" (2026-03-05)
  - Interview with prospect: "I'd use it if the banner sounded like a person" (2026-03-14)
- **Strength:** Medium (4 sources, 4 types, recent)
- **Addressability:** Yes — alternative copy template
- **Derived Backlog Item:** "Add plain-language cookie banner copy variant"
- **Biases:** Selection (self-reported), anglophone (3/4 sources)

This pattern goes into a Discovery Insight output; the derived item goes to the Lead PO for scoring.
