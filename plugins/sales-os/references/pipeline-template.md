# Pipeline File Template

The pipeline is a plain markdown file the user owns (default `~/sales/pipeline.md`). It is the single working copy; CRMs are read-only sources. Keep it small enough to read in one glance — a solopreneur rarely needs more than 15-25 open deals.

## Structure

```markdown
# Sales Pipeline — {Business Name}
_Last updated: {ISO date}_

## Open Deals

| Deal | Company | Contact | Stage | Value | Next action | Due | Last touch | Notes |
|---|---|---|---|---|---|---|---|---|
| Aurora rollout | Acme Studio | Jordan Reyes (Head of Ops) | proposal | EUR 8,000 | Send tiered proposal | 2026-07-11 | 2026-07-08 | Budget confirmed |

## Parked / Nurture

| Deal | Company | Contact | Reason parked | Revisit on |
|---|---|---|---|---|

## Win/Loss Log

| Date | Deal | Company | Value | Outcome | Reason category | Reason detail |
|---|---|---|---|---|---|---|
| 2026-06-30 | Northwind audit | Northwind Ltd | EUR 4,500 | lost | price | Chose cheaper freelancer |
```

## Rules

- **Every open deal has a next action with a due date.** No exceptions — a deal without one is flagged in every review.
- **Stages** come from config `pipeline.stages`; don't invent new ones ad hoc. Move a deal by editing its Stage cell and updating Last touch.
- **Values** in the configured currency; use expected engagement value, not fantasy lifetime value.
- **Reason categories** for the win/loss log (fixed vocabulary, so patterns emerge): `price` · `timing` · `fit` · `competitor` · `feature-gap` · `no-decision` · `relationship` · `other`.
- **Parked ≠ lost.** Deals that may revive go to Parked with a concrete revisit date; the stale sweep ignores them until then.
- **CSV variant:** if the user prefers CSV, same columns, one file per table (`pipeline.csv`, `winloss.csv`). Markdown is the default because it's human-readable in any editor.

## Stale-deal thresholds

| Stage | Default staleness trigger |
|---|---|
| lead / contacted | no touch in `stale_after_days` (default 14) |
| qualified / proposal | no touch in 10 days |
| negotiation | no touch in 7 days |

Suggested escalation per stale flag: 1st flag → nudge · 2nd flag → new-angle follow-up · 3rd flag → breakup email or park.
