# CFO Routing Table (full)

Read this only when the quick-reference table in SKILL.md leaves the route ambiguous.

## Fast-mode routes (one agent, relay output)

| Example phrasings | Agent |
|---|---|
| "Here's my bank export for June" · "categorize these" · "what did I spend on software?" · "monthly bookkeeping" · "is this charge a duplicate?" · "clean up my expenses" | `finance-os:bookkeeper` |
| "Invoice Acme Studio for the Aurora onboarding" · "draft invoice, 3 days at my day rate" · "they still haven't paid" · "write a payment reminder" · "who owes me money?" · "mark invoice 2026-014 paid" | `finance-os:invoicing` |
| "What's my runway?" · "13-week forecast" · "burn rate" · "can I afford a new laptop?" · "what if my biggest client leaves?" · "what if I hire a contractor at 2k/month?" | `finance-os:cashflow-forecaster` |
| "When is VAT due?" · "how much should I set aside for taxes?" · "quarterly prepayment estimate" · "what does my accountant need for Q2?" · "I'm registering in a new country — what deadlines change?" | `finance-os:tax-planner` |
| "What should I charge for X?" · "am I too cheap?" · "what do competitors charge?" · "client wants 20% off" · "should I move to tiers?" | `finance-os:pricing-analyst` |

## Multi-agent routes (announce agents before running)

| Request pattern | Route | Model |
|---|---|---|
| "Should I raise prices?" (they'll want the cash consequence) | `pricing-analyst` (impact estimate) → `cashflow-forecaster` (price-change scenario with those numbers) | Pipeline |
| "Can I afford to hire, considering taxes coming up?" | `tax-planner` (payment dates/amounts) → `cashflow-forecaster` (hire scenario incl. tax outflows) | Pipeline |
| "Monthly finance review" / "financial report" / "how is the business doing?" | Parallel: `bookkeeper` (month summary, needs pasted data), `invoicing` (AR aging), `cashflow-forecaster` (runway). Add `tax-planner` if a deadline falls within ~6 weeks. Synthesize per output-formats.md | Collaborative |
| "Get me ready for year-end" | `bookkeeper` (tidy-up) → `tax-planner` (accountant pack) | Pipeline |
| "New big client signed — update everything" | `invoicing` (first invoice) + `cashflow-forecaster` (revenue added) | Collaborative |

## Routing tiebreakers

- Pasted transaction data anywhere in the request → `bookkeeper` is involved.
- Mentions a specific client + money owed → `invoicing`, even if phrased as "cash flow problem".
- "How much tax" → `tax-planner`; "how much should I charge" → `pricing-analyst`.
- Pure "what if" questions about the future → `cashflow-forecaster`, even when the trigger is a price or hire (pipeline only if they also want the pricing/tax analysis itself).
- When still ambiguous: ask one short clarifying question instead of fanning out.
