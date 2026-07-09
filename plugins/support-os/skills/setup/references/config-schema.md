# Support-OS Configuration Schema

## File location

`~/.claude/plugins/data/support-os/config.json`

## Top level

| Field | Type | Required | Description |
|---|---|---|---|
| `active_profile` | string | yes | Slug of the currently active product profile |
| `profiles` | object | yes | Map of profile slug → profile object |

## Profile object

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `product_name` | string | yes | — | Product name |
| `pitch` | string | no | — | One-line pitch |
| `domain` | string | no | — | Primary domain |
| `channels` | string[] | yes | `["email"]` | Where support arrives: `email`, `chat`, `social`, `community`, `other` |
| `helpdesk` | Helpdesk | no | `{tool: "none"}` | Helpdesk tool metadata (CSV interpretation only — never credentials) |
| `voice` | Voice | yes | — | Brand voice for replies and articles |
| `sla` | Sla | yes | — | Response-time expectations |
| `refund_policy` | RefundPolicy | yes | — | Policy summary and escalation threshold |
| `escalation` | Escalation | no | see below | Decisions reserved for the user |
| `kb` | Kb | no | `{}` | Help-center location and inventory |
| `cross_os` | CrossOS | yes | all `false` | Sibling plugin availability flags |
| `created_at` / `updated_at` | string | no | — | ISO dates |

### Helpdesk

| Field | Type | Description |
|---|---|---|
| `tool` | string | `none`, `zendesk`, `helpscout`, `freshdesk`, `intercom`, `plain-inbox`, `other` |
| `export_format` | string | Usually `csv`; helps agents map export columns |

### Voice

| Field | Type | Description |
|---|---|---|
| `tone` | string | 2-3 adjectives or one sentence |
| `signoff` | string | e.g. "— Sam, Aurora founder" |
| `use_marketing_os_voice` | boolean | If true and marketing-os is installed, its brand voice wins |
| `avoid` | string[] | Words/phrases never to use (optional) |

### Sla

| Field | Type | Default | Description |
|---|---|---|---|
| `first_response_hours` | number | 24 | Target first response |
| `resolution_hours` | number | 72 | Target resolution |
| `business_days_only` | boolean | true | Whether targets count business days only |

### RefundPolicy

| Field | Type | Description |
|---|---|---|
| `summary` | string | 1-3 sentence plain-language policy |
| `window_days` | number | Refund window (e.g. 14) |
| `user_decides_above` | string | Threshold/conditions above which the user decides personally (amount, annual plans, repeat refunds) |

### Escalation

| Field | Type | Default |
|---|---|---|
| `user_decides` | string[] | `["refunds above policy threshold", "legal threats", "security reports"]` |

### Kb

| Field | Type | Description |
|---|---|---|
| `location` | string | Help-center URL or repo path |
| `inventory_path` | string | Optional local markdown file mirroring the article inventory |

### CrossOS

| Field | Type | Default | Description |
|---|---|---|---|
| `po_os` | boolean | false | po-os installed → insights-analyst emits Discovery Insights |
| `marketing_os` | boolean | false | marketing-os installed → reuse its brand voice |

## Example

```json
{
  "active_profile": "aurora",
  "profiles": {
    "aurora": {
      "product_name": "Aurora",
      "pitch": "Compliance tracking for small teams",
      "domain": "aurora.example.com",
      "channels": ["email", "chat"],
      "helpdesk": { "tool": "helpscout", "export_format": "csv" },
      "voice": {
        "tone": "friendly, direct, no corporate filler",
        "signoff": "— Sam, Aurora founder",
        "use_marketing_os_voice": true,
        "avoid": ["per our policy", "valued customer"]
      },
      "sla": { "first_response_hours": 24, "resolution_hours": 72, "business_days_only": true },
      "refund_policy": {
        "summary": "Full refund within 14 days of purchase, no questions asked. After 14 days, pro-rated at our discretion.",
        "window_days": 14,
        "user_decides_above": "any refund after the 14-day window, or on annual plans"
      },
      "escalation": {
        "user_decides": ["refunds above policy threshold", "legal threats", "security reports"]
      },
      "kb": { "location": "https://aurora.example.com/help", "inventory_path": "" },
      "cross_os": { "po_os": true, "marketing_os": true },
      "created_at": "2026-07-09",
      "updated_at": "2026-07-09"
    }
  }
}
```
