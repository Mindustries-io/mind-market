# Pricing Frameworks for Solopreneurs

Playbook reference for the `pricing-analyst` agent.

## 1. Value-Based Framing

Anchor price to customer outcome, not your time or costs.

1. **Quantify the outcome** — what does the customer gain or stop losing? (hours saved × their loaded rate, revenue enabled, risk avoided, e.g. "Aurora saves a compliance manager ~6 h/week").
2. **10x rule of thumb** — price at roughly 10–20% of quantified first-year value; if you can't defend that ratio, either the value story or the price is wrong.
3. **Cost floor** — value sets the ceiling; your economics set the floor: `floor = (desired annual profit + annual costs + tax reserve) / realistic billable capacity`. Never price below the floor to win volume.
4. **Framing ladder** — hourly → daily → project → retainer → value/outcome. Each step up decouples income from hours; recommend the highest rung the relationship supports.

## 2. Packaging Patterns

- **Good / Better / Best** — 3 tiers, middle tier is the target (price the top tier to make the middle look reasonable). Differentiate on outcomes and access, not feature nickel-and-diming.
- **Anchor tier** — a deliberately premium option (2.5–4x middle) that few buy but that re-anchors perception.
- **Productized service** — fixed scope, fixed price, fixed timeline; best conversion for solo services.
- **Retainer** — recurring access/capacity; price at a discount to ad-hoc *only* in exchange for commitment length or prepayment.

## 3. Competitor Benchmarking (WebSearch)

1. Identify 3–5 direct alternatives (config `pricing.competitors` first; otherwise search "{category} pricing").
2. Capture: public price points, billing unit (seat/usage/flat), tier boundaries, positioning language.
3. Place the user: cheapest / mid / premium. Note that solo operators should rarely anchor cheapest — support and trust don't scale down.
4. Cite sources with dates; public pricing pages change — flag stale data (>6 months).

## 4. Price-Change Impact Estimate

Feed results into the cash-flow model (hand to `cashflow-forecaster` or apply its price-change scenario):

- `new MRR ≈ old MRR × (1 + Δprice) × (1 − churn_from_increase)`
- Elasticity default for B2B SaaS/services: 1–3% extra churn per 10% increase for grandfathered-then-migrated bases; near zero for new-customers-only increases. Always label these as assumptions.
- Recommend sequencing: new customers first → existing at renewal with 30–60 day notice → grandfather strategic accounts explicitly.
- Break-even check: an increase of X% tolerates losing up to `X/(100+X)` of customers before revenue falls (e.g. +20% tolerates ~16.7% loss).

## 5. Discount Policy Guardrails

- **Never discount without getting something**: longer commitment, prepayment, case study, referral, reduced scope.
- Hard floor: max discount from config `pricing.max_discount_pct` (default 15%). Below floor → change scope instead of price.
- No permanent discounts — every discount has an expiry written into the offer.
- Time-pressure discounts ("sign this week") erode trust in B2B; prefer bonuses (extra deliverable) over price cuts.
- Track every discount given (memory: `fin: {BUSINESS} discount granted ...`) — creeping average discount is a pricing-integrity smell.

## Output Expectations

Pricing recommendations always include: current vs. proposed structure, benchmark table with sources, revenue impact estimate with labeled assumptions, risks, and a stepwise rollout plan. Final pricing decisions are the user's; see `SAFETY.md`.
