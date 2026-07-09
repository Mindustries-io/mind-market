# Finance OS Config Schema

Location: `~/.claude/plugins/data/finance-os/config.json`. All fields optional unless marked required — agents degrade gracefully per the startup protocol.

```json
{
  "version": 1,
  "business": {
    "name": "Acme Studio",                       // required
    "legal_form": "sole proprietorship",
    "entity_type": "sole_prop",                  // sole_prop | llc_equivalent | corporation | other
    "currency": "EUR",                           // required, ISO 4217
    "address": "Example Street 1, 00100 Example City",
    "email": "billing@aurora.example.com",
    "tax_ids": [
      { "label": "VAT ID", "value": "XX123456789", "jurisdiction": "XX" }
    ]
  },
  "tax": {
    "jurisdictions": {
      "primary": "XX",                           // ISO 3166-1 alpha-2 (+ optional region)
      "others": []
    },
    "vat": {
      "registered": true,
      "rate": 22,                                // percent, standard rate
      "scheme": "standard",                      // standard | cash | flat_rate | small_business_exempt
      "filing_frequency": "quarterly"            // monthly | quarterly | annual
    },
    "calendar": [
      { "date": "2026-07-31", "jurisdiction": "XX", "obligation": "VAT Q2 return + payment", "est_amount": null }
    ]
  },
  "revenue": {
    "model": "mixed",                            // services | products | mixed
    "recurring": [
      { "name": "Aurora Pro subscriptions", "amount": 2400, "frequency": "monthly", "billing_day": 1, "churn_pct": 2 },
      { "name": "Retainer — consulting", "amount": 1500, "frequency": "monthly", "billing_day": 1 }
    ],
    "rates": { "day_rate": 800, "hourly_rate": 110, "typical_project": 6000 }
  },
  "costs": {
    "recurring": [
      { "name": "Hosting", "amount": 180, "frequency": "monthly", "billing_day": 5, "category": "cogs:hosting" },
      { "name": "Owner draw", "amount": 3000, "frequency": "monthly", "billing_day": 28, "category": "owner:draw" }
    ]
  },
  "invoicing": {
    "payment_terms_days": 14,
    "numbering": "{YYYY}-{seq}",
    "next_number": 15,
    "payment_details": "IBAN XX00 0000 0000 0000",
    "footer_notes": "",
    "chase_days": [3, 14, 30]                    // days after due date for chase steps 1-3
  },
  "cashflow": {
    "min_buffer": "1_month_costs",               // number (amount) or "1_month_costs"
    "payment_lag_days": 7
  },
  "bookkeeping": {
    "custom_categories": []                      // extends references/category-taxonomy.md, e.g. "opex:studio-rent"
  },
  "pricing": {
    "competitors": [
      { "name": "ExampleCompetitor", "pricing_url": "https://competitor.example.com/pricing" }
    ],
    "max_discount_pct": 15
  },
  "cross_os": {
    "legal_os": false,
    "marketing_os": false,
    "po_os": false
  }
}
```

## Data files (same directory, maintained by agents with user confirmation)

| File | Owner | Shape |
|---|---|---|
| `invoices.json` | invoicing | `{ "invoices": [...], "updated_at": "" }` — see `references/invoice-templates.md` |
| `forecast.json` | cashflow-forecaster | `{ "assumptions": {...}, "updated_at": "" }` — see `references/forecast-model.md` |

## Conventions

- Jurisdiction identifiers use ISO 3166-1 alpha-2 codes — the same convention as legal-os, so `cross_os.legal_os` reuse maps 1:1.
- Amounts are plain numbers in `business.currency`; no cents-as-integers.
- Never store credentials, API keys, or logins anywhere in this config.
