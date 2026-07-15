---
name: delivery-scorecard
description: Build or refresh the delivery department scorecard — CSAT, delivery-margin deviation, PoC-to-Production conversion, expansion revenue %, and upsells — rolled up across the project portfolio with per-account detail and RAG statuses. Use whenever the user mentions the delivery scorecard, portfolio health, delivery KPIs, department metrics, margin deviation, PoC conversion, expansion percentage, or asks how delivery is performing this week/month/quarter. Also trigger for weekly delivery reviews and exec/board reporting on the delivery organization.
---

# Delivery Scorecard

## Startup

Read `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md` and follow it before any task: resolve `<DATA_DIR>`, load `config.json` (offering the inline quick-setup if missing), and honour the data-sensitivity rules. Present the scorecard as a Markdown document, and persist a structured snapshot of the metrics (per-account values, roll-ups, RAG statuses, date) by appending it to the `history` array in `<DATA_DIR>/scorecard.json`. The Markdown is the report; the JSON is the record.

Produce the operating scorecard for a services delivery organization. It is both a management instrument (weekly/monthly review) and the evidence base for exec and board reporting — so numbers must be traceable to their source, and judgements must be separated from data.

## The five core metrics

| Metric | Definition | Default target |
|---|---|---|
| CSAT | Latest customer satisfaction per active account, portfolio average | ≥ 4.0 / 5.0 |
| Delivery margin deviation | Actual margin vs. planned margin, per project and weighted portfolio | Within ±X pp of plan (user sets X) |
| PoC-to-Production conversion | % of PoCs/pilots in the period that converted to production engagements | > 30% |
| Expansion revenue % | Share of total revenue from existing-account expansion vs. new logos | > 70% |
| Upsells | Count and value of qualified expansion opportunities and closed upsells in the period | 2+ qualified opps per account per quarter |

Targets are defaults — always ask whether the organization has agreed different targets, and use those. If a metric can't be computed from available data, show it as `NO DATA` with what's needed to compute it; never estimate a number that will be read as a measurement.

## Gathering inputs

Accept data in whatever form exists: pasted numbers, CSV/XLSX exports, or connected tools (project tracker, CRM, finance sheets). For spreadsheet inputs, compute the metrics programmatically rather than eyeballing. When a previous scorecard is provided, compute period-over-period deltas — trend is more decision-relevant than the point value.

## Output format

A single Markdown document:

```markdown
# Delivery Scorecard — [period]
**Prepared:** [date] · **Prior period:** [link/ref or "first edition"]

## 1. Headline
| Metric | Actual | Target | Δ vs prior | RAG |
|---|---|---|---|---|
(the five core metrics)
One-paragraph verdict: overall portfolio RAG and the single most important thing leadership should act on.

## 2. Per-account / per-project detail
| Account | Project | CSAT | Margin plan | Margin actual | Deviation | Health | Notes |
|---|---|---|---|---|---|---|---|

## 3. Risks and escalations
Open Significant/Catastrophic risks with owner and status. State explicitly if there are none.
% of issues resolved at team level (target ≥ 90%).

## 4. Expansion engine
Qualified opportunities by account, PoC pipeline and conversions, upsells closed in period.

## 5. Actions
What the numbers demand: numbered, owned, dated.
```

## RAG discipline

Green = on target. Yellow = off target with a credible recovery path. Red = off target, no agreed recovery path. A metric with no data is grey/`NO DATA`, not green — undetected problems are the expensive kind. When in doubt between two colours, pick the worse one and say why; scorecards fail through optimism, not pessimism.

## Judgement vs. data

Keep measured values, derived values, and opinions visually distinct: measurements cite their source, derivations show the formula, and any qualitative call (e.g. account health) is attributed ("ADM assessment", "CDO judgement"). This is what lets the scorecard survive scrutiny in a board pack.
