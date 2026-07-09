# Sales OS Configuration Schema

## File Location
`~/.claude/plugins/data/sales-os/config.json`

## Top Level

| Field | Type | Required | Description |
|---|---|---|---|
| `active_profile` | string | yes | Slug of the currently active business profile |
| `profiles` | object | yes | Map of profile slug → profile object |

## Profile Object

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `business_name` | string | yes | - | Display name of the business |
| `offer` | object | yes | - | What is sold (see below) |
| `pricing` | object | yes | - | Rates and floors (see below) |
| `icp` | object | no | - | Ideal customer profile (see below) |
| `pipeline` | object | no | - | Pipeline settings (see below) |
| `outreach` | object | no | - | Outreach voice + guardrails (see below) |
| `proposal` | object | no | - | Proposal/SOW defaults (see below) |
| `crm` | object | no | `{"type":"none"}` | CRM situation (see below) |
| `cross_os` | object | no | all false | Installed sibling OS flags (see below) |
| `created_at` / `updated_at` | string | no | - | ISO dates |

### offer

| Field | Type | Default | Description |
|---|---|---|---|
| `services` | object[] | [] | `{name, description, unit}` — unit: hourly/daily/fixed/retainer |
| `positioning` | string | "" | One sentence: "I help X do Y" |

### pricing

| Field | Type | Default | Description |
|---|---|---|---|
| `currency` | string | "EUR" | ISO 4217 code |
| `rates` | object[] | [] | `{service, unit, rate}` |
| `minimum_engagement` | number | 0 | Floor value for any engagement |
| `discount_floor_pct` | number | 0 | Max discount before flagging |

### icp

| Field | Type | Default |
|---|---|---|
| `description` | string | "" |
| `industries` | string[] | [] |
| `company_size` | string | "" |
| `buyer_roles` | string[] | [] |
| `geographies` | string[] | [] |
| `red_flags` | string[] | [] |

### pipeline

| Field | Type | Default |
|---|---|---|
| `stages` | string[] | ["lead","contacted","qualified","proposal","negotiation","won","lost"] |
| `file_path` | string | "~/sales/pipeline.md" |
| `stale_after_days` | number | 14 |
| `review_day` | string | "monday" |

### outreach

| Field | Type | Default |
|---|---|---|
| `tone` | string | "friendly-direct" |
| `sign_off` | string | "" |
| `channels` | string[] | ["email"] |
| `daily_send_cap` | number | 10 |
| `do_not_contact` | string[] | [] — emails or domains, honored absolutely |

### proposal

| Field | Type | Default |
|---|---|---|
| `standard_terms` | string | "" — payment terms, revision rounds, etc. |
| `payment_terms` | string | "50% upfront, 50% on delivery" |
| `validity_days` | number | 30 |
| `tiers_default` | number | 3 |
| `always_out_of_scope` | string[] | [] |

### crm

| Field | Type | Default | Description |
|---|---|---|---|
| `type` | string | "none" | none / spreadsheet / hubspot / pipedrive / attio |
| `export_path` | string | "" | Where the user drops CSV exports |
| `notes` | string | "" | e.g. "MCP server connected, read-only" — never credentials |

### cross_os

| Field | Type | Default | Effect when true |
|---|---|---|---|
| `marketing_os` | boolean | false | Outreach reuses marketing-os brand voice |
| `legal_os` | boolean | false | Custom contract terms escalate to legal-os contract-manager |
| `po_os` | boolean | false | Win/loss reasons emitted for po-os discovery-voc |

## Example (fictional)

```json
{
  "active_profile": "acme-studio",
  "profiles": {
    "acme-studio": {
      "business_name": "Acme Studio",
      "offer": {
        "services": [{ "name": "Compliance dashboard build", "description": "Custom Aurora-based compliance tracking setup", "unit": "fixed" }],
        "positioning": "I help SME ops teams automate compliance tracking with Aurora."
      },
      "pricing": { "currency": "EUR", "rates": [{ "service": "Compliance dashboard build", "unit": "fixed", "rate": 8000 }], "minimum_engagement": 2000 },
      "pipeline": { "stages": ["lead", "contacted", "qualified", "proposal", "negotiation", "won", "lost"], "file_path": "~/sales/pipeline.md", "stale_after_days": 14 },
      "outreach": { "tone": "friendly-direct", "channels": ["email", "linkedin"], "do_not_contact": [] },
      "crm": { "type": "none" },
      "cross_os": { "marketing_os": true, "legal_os": false, "po_os": true }
    }
  }
}
```
