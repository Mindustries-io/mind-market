# Delivery OS — config.json schema

```json
{
  "organization": {
    "name": "Acme Corp",
    "unit": "Delivery",
    "currency": "EUR"
  },
  "accounts": {
    "tier_definitions": {
      "tier1": "Strategic accounts — e.g. > 500k ARR or named key client",
      "tier2": "Growth accounts",
      "tier3": "Maintain"
    }
  },
  "kpi_targets": {
    "csat_min": 4.0,
    "delivery_margin_deviation_max_pct": 5,
    "poc_to_production_min_pct": 30,
    "expansion_revenue_min_pct": 70,
    "upsell_opportunities_per_account_per_quarter": 2
  },
  "rag_thresholds": {
    "csat": { "green_min": 4.0, "amber_min": 3.5 }
  },
  "review_cadence": {
    "scorecard": "weekly",
    "adp_refresh": "monthly",
    "exec_report": "monthly"
  },
  "connectors": {
    "enabled": true,
    "preferred": []
  },
  "created_at": "2026-07-15",
  "updated_at": "2026-07-15"
}
```

`rag_thresholds` is optional: omit it entirely to use the skills' default thresholds; include only the metrics you want to override.

Data files created alongside config.json:
- `scorecard.json` — latest scorecard snapshot plus history array
- `adp/<account-slug>.md` — one Account Development Plan per account
