# 13-Week Rolling Cash-Flow Forecast Model

Structure used by the `cashflow-forecaster` agent. The model is a simple weekly waterfall — deliberately spreadsheet-shaped so the user can copy it out.

## Inputs

1. **Opening cash** — current business bank balance(s), user-provided. Ask for the as-of date.
2. **Recurring revenue** — from config `revenue.recurring` (e.g. Aurora subscriptions: MRR, expected churn %) plus retainers with their billing day.
3. **Expected one-off inflows** — outstanding invoices from the invoicing ledger (weight by likelihood: current 95%, overdue <30d 80%, 30–60d 50%, >60d 25% — adjustable), signed-but-unbilled work, tax refunds.
4. **Recurring costs** — from config `costs.recurring` (software, hosting, insurance, contractors, owner draw) with their billing week/month.
5. **Known one-offs** — tax prepayments and VAT remittances (from tax-planner or config `tax.calendar`), annual renewals, planned purchases.

## Weekly Waterfall

For each of the next 13 weeks (W1 = current week, Monday-start):

```
Opening balance (W_n)
+ Inflows:  recurring revenue landing this week
            + weighted invoice collections
            + other one-off inflows
- Outflows: recurring costs due this week
            + tax payments due this week
            + one-off outflows
= Closing balance (W_n)  →  Opening balance (W_n+1)
```

Timing rules: place inflows in the week payment is *expected to land* (invoice due date + typical payment lag, default +7 days), not the invoice date. Place VAT/tax on statutory due dates. Monthly items land in the week containing their billing day.

## Derived Metrics

- **Net burn / net gain per week** and 4-week trailing average.
- **Runway** = weeks until closing balance < safety buffer (config `cashflow.min_buffer`, default 1 month of average outflows). If never within 13 weeks, extrapolate: `runway_months ≈ (cash − buffer) / avg monthly net burn` (only when net-negative; otherwise report "cash-flow positive at current run rate").
- **Low-water mark** — the minimum weekly closing balance and its date. This matters more than the ending balance.

## Scenario Toggles

Run the base model, then re-run with deltas. Standard scenarios:

| Scenario | Delta |
|---|---|
| **Lose top client** | Remove largest recurring revenue item from its next billing date; keep their outstanding invoices at 50% weight |
| **Hire contractor** | Add monthly cost (user gives rate) from a start week |
| **Price change** | Apply % to recurring revenue after notice period; optionally add churn assumption (default: 1–3% extra churn per 10% increase — flag as an assumption, cross-check with pricing-analyst) |
| **Late-payment stress** | Shift all invoice collections +4 weeks |
| Custom | Any user-supplied delta |

Report each scenario as: new runway, new low-water mark, first week that breaches the buffer.

## Output Table Shape

```
| Week (w/c) | Inflows | Outflows | Net | Closing |
|---|---|---|---|---|
| W1 2026-07-06 | 4,200 | 3,100 | +1,100 | 13,400 |
| ...13 rows... |
```

Follow with: runway statement, low-water mark, top 3 drivers, assumptions list (every guessed number must appear here), and scenario comparison table when scenarios were requested.

## Data Persistence

Persist model assumptions to `<DATA_DIR>/forecast.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`) — recurring items, weights, buffer — so the next run only needs the new opening balance and changes. Show the user what changed since last run.
