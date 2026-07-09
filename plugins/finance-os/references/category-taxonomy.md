# Standard Solopreneur Expense Category Taxonomy

Default chart of categories for the `bookkeeper` agent. The user's config may override or extend these (`bookkeeping.custom_categories`). Keep category names stable across months so reports are comparable.

## Income

| Category | Typical matches |
|---|---|
| `income:services` | Client invoices, consulting fees, retainers |
| `income:products` | SaaS subscriptions, digital products, licenses (e.g. Stripe payouts for Aurora plans) |
| `income:affiliate` | Affiliate/referral commissions |
| `income:interest` | Bank interest, savings yield |
| `income:other` | Refunds received, grants, one-offs |

## Cost of Delivery

| Category | Typical matches |
|---|---|
| `cogs:hosting` | AWS, Vercel, Hetzner, domain-adjacent infra serving customers |
| `cogs:payment-fees` | Stripe/PayPal fees, currency conversion fees |
| `cogs:subcontractors` | Freelancers doing billable client work |
| `cogs:licenses` | Per-customer or usage-based licenses resold in the product |

## Operating Expenses

| Category | Typical matches |
|---|---|
| `opex:software` | SaaS tools (email, design, project management, AI tools) |
| `opex:marketing` | Ads, sponsorships, SEO tools, newsletter platform |
| `opex:professional` | Accountant, lawyer, notary, business consulting |
| `opex:insurance` | Professional liability, health (where business-deductible) |
| `opex:banking` | Account fees, card fees (not payment processing) |
| `opex:office` | Coworking, home-office share, furniture, supplies |
| `opex:equipment` | Hardware, devices (flag possible capitalization above local threshold) |
| `opex:travel` | Flights, hotels, mileage, per-diems for business trips |
| `opex:meals` | Business meals (flag: often partially deductible) |
| `opex:education` | Courses, books, conferences |
| `opex:phone-internet` | Phone plan, internet (flag business-use percentage) |
| `opex:memberships` | Professional associations, chambers |

## Taxes, Owner & Transfers

| Category | Typical matches |
|---|---|
| `tax:vat-payment` | VAT/GST remittance to the tax authority |
| `tax:income-prepayment` | Income/corporate tax prepayments |
| `tax:social-contributions` | Self-employment social security contributions |
| `owner:draw` | Owner withdrawals / salary to self |
| `owner:contribution` | Personal money put into the business |
| `transfer:internal` | Moves between own accounts — NOT income or expense |

## Categorization Rules

1. **Transfers are not transactions.** Anything matching `transfer:internal` must be excluded from income/expense totals.
2. **Confidence tiers.** Mark each suggestion High / Medium / Low. Low-confidence items go to a review list — never silently guess on ambiguous merchants.
3. **Deductibility flags** (`meals`, `phone-internet`, `equipment`, mixed-use items) are informational only — the accountant decides. See `SAFETY.md`.
4. **Duplicates:** same amount + same counterparty within 3 days → flag, don't delete.
5. **Anomalies:** new merchant over ~2x the category's median, category totals ±50% vs. trailing 3-month average, or a subscription price increase → flag in the anomaly section.
