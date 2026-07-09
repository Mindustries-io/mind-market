# CFO Output Formats (multi-agent synthesis)

Used only when the CFO combines multiple specialists. Fast-mode outputs use each agent's own format.

## Monthly Finance Review / Financial Report

```
# {BUSINESS} — Finance Review, {period}

**Headline:** {one sentence: cash position, runway, biggest item needing action}

## Cash
- Position: {CURRENCY} {balance} (as of {date}) · Runway: {n} weeks · Low-water mark: {amount}, week of {date}
- {2-3 bullets: biggest inflow/outflow drivers this period}

## P&L Snapshot (from bookkeeper)
| | This month | Prior month | Δ |
|---|---|---|---|
| Income | | | |
| Expenses | | | |
| Net | | | |
Top expense categories: {top 3 with amounts}
Flags: {anomalies/duplicates count — link to review list}

## Receivables (from invoicing)
Outstanding: {CURRENCY} {total} · Aging: current {x} / 1-30 {y} / 31-60 {z} / 60+ {w}
Next chase actions: {list}

## Tax (when tax-planner ran)
Next deadlines: {date — obligation — est. amount}
Reserve check: {recommended vs. actually set aside, if known}

## Assumptions (merged, labeled)
- {every estimated/guessed number from any specialist}

## Actions
1. {most important next step}
2. ...
```

## Pipeline Result (e.g. price change → forecast)

```
# {question, e.g. "Raise prices 15%?"}

**Recommendation:** {one sentence}

## Analysis ({upstream agent})
{condensed key numbers — benchmark or tax table, not the full specialist output}

## Cash impact ({downstream agent})
| Scenario | Runway | Low-water mark | First buffer breach |
|---|---|---|---|
| Base | | | |
| {scenario} | | | |

## Assumptions
{merged, labeled}

## Next steps
{rollout / confirmation steps}
```

## Rules

- One assumptions section per report — merge, never scatter.
- Every external figure keeps its source citation from the specialist.
- Reports end with actions, not summaries.
- Close with the standing note: "Prepared with Finance OS — verify figures with your accountant before filing or committing." (single line; full text in SAFETY.md).
