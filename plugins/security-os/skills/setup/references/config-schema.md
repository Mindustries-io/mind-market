# Security OS Configuration Schema

## File Location
`<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`). Example default location — location 3 of the resolution order: `~/.claude/plugins/data/security-os/config.json`

## Top Level

| Field | Type | Required | Description |
|---|---|---|---|
| `active_profile` | string | yes | Slug of the currently active profile |
| `profiles` | object | yes | Map of profile slug -> profile object |

## Profile Object

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `org_name` | string | yes | - | Business/product name |
| `domain` | string | no | - | Primary domain (e.g. `aurora.example.com`) |
| `product_summary` | string | no | - | One-line description of the product |
| `team_size` | string | no | "solo" | "solo", "2-5", "6+" |
| `stack` | object | yes | - | Stack inventory (see below) |
| `crown_jewels` | string[] | yes | [] | 1-5 most critical data/systems |
| `existing_controls` | object | no | - | Known controls (see below) |
| `risk_appetite` | object | no | - | Risk thresholds (see below) |
| `connectors` | object | no | - | MCP connector awareness (see below) |
| `cross_os` | object | no | - | Sibling plugin flags (see below) |
| `created_at` | string | no | - | ISO date of profile creation |
| `updated_at` | string | no | - | ISO date of last update |

### Stack Object

| Field | Type | Default | Description |
|---|---|---|---|
| `languages` | string[] | [] | Languages/frameworks (e.g. "typescript/nextjs", "python/django") |
| `hosting` | string[] | [] | Hosting/cloud providers (e.g. "vercel", "aws") |
| `data_stores` | string[] | [] | Where production data lives (e.g. "postgres@neon", "s3") |
| `code_hosting` | string | "" | e.g. "github", "gitlab" |
| `ci` | string | "" | e.g. "github-actions" |
| `saas_tools` | string[] | [] | SaaS with access to code/data/money (email, payments, analytics, AI tools) |

### Existing Controls Object

| Field | Type | Default | Description |
|---|---|---|---|
| `mfa` | string | "unknown" | Free-form: where MFA/passkeys are enabled |
| `password_manager` | boolean\|string | "unknown" | Whether/which one |
| `backups` | string | "unknown" | What is backed up, where, restore-tested? |
| `secrets_management` | string | "unknown" | e.g. "env files", "1password", "vault", "cloud secret manager" |
| `monitoring` | string | "unknown" | Alerts/logging in place |
| `notes` | string | "" | Anything else |

### Risk Appetite Object

| Field | Type | Default | Description |
|---|---|---|---|
| `level` | string | "moderate" | "conservative", "moderate", "aggressive" |
| `escalation_threshold` | string | "high" | Severity requiring explicit user decision: "medium", "high", "critical" |

### Connectors Object

```json
"connectors": {
  "enabled": true,
  "preferred": []
}
```

| Field | Type | Default | Description |
|---|---|---|---|
| `enabled` | boolean | true | When false, agents never probe for MCP connectors and use exports only |
| `preferred` | string[] | [] | Optional connector/product names to look for first |

### Cross-OS Object

| Field | Type | Default | Description |
|---|---|---|---|
| `legal_os_installed` | boolean | false | legal-os plugin available — enables vendor-risk handoff (`sec:vendor:*` memory keys) and incident GDPR-track handoff |

## Example

```json
{
  "active_profile": "aurora",
  "profiles": {
    "aurora": {
      "org_name": "Acme Studio",
      "domain": "aurora.example.com",
      "product_summary": "Aurora — compliance-tracking SaaS",
      "team_size": "solo",
      "stack": {
        "languages": ["typescript/nextjs"],
        "hosting": ["vercel"],
        "data_stores": ["postgres@neon"],
        "code_hosting": "github",
        "ci": "github-actions",
        "saas_tools": ["stripe", "resend", "plausible"]
      },
      "crown_jewels": ["customer PII in postgres", "stripe account", "founder email account"],
      "existing_controls": {
        "mfa": "passkeys on github and email; totp on stripe",
        "password_manager": "1password",
        "backups": "neon PITR; never restore-tested",
        "secrets_management": "vercel env vars",
        "monitoring": "vercel alerts only"
      },
      "risk_appetite": { "level": "moderate", "escalation_threshold": "high" },
      "connectors": { "enabled": true, "preferred": [] },
      "cross_os": { "legal_os_installed": true },
      "created_at": "2026-07-09",
      "updated_at": "2026-07-09"
    }
  }
}
```
