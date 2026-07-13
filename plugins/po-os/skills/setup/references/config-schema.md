# PO-OS Configuration Schema

## File Location
`<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`). The example default location — location 3 of the resolution order — is `~/.claude/plugins/data/po-os/config.json`.

## Schema

| Field | Type | Required | Description |
|---|---|---|---|
| `active_profile` | string | yes | Slug of the currently active product profile |
| `profiles` | object | yes | Map of profile slug → profile object |
| `connectors` | object | no | Optional MCP connector awareness (see below) |

### Connectors Object

```json
"connectors": {
  "enabled": true,
  "preferred": []
}
```

| Field | Type | Default | Description |
|---|---|---|---|
| `enabled` | boolean | true | When false, agents never probe for connectors and use exports only |
| `preferred` | string[] | [] | Optional connector/product names to look for first |

## Profile Object

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `product_name` | string | yes | — | Product name |
| `pitch` | string | yes | — | One-line pitch |
| `domain` | string | yes | — | Primary domain |
| `stage` | string | yes | "ga" | `pre-launch`, `early-access`, `ga`, `scaling` |
| `repo` | string | no | — | GitHub `owner/repo` for backlog |
| `personas` | Persona[] | yes | [] | Target personas (see below) |
| `markets` | object | yes | — | `primary`, `secondary` arrays of ISO codes |
| `covered_frameworks` | string[] | yes | [] | Frameworks the product addresses |
| `local_frameworks` | string[] | no | [] | Non-EU frameworks |
| `competitors` | Competitor[] | no | [] | Competitive landscape |
| `status_quo_alternative` | string | no | — | What prospects do without any tool |
| `backlog` | Backlog | yes | — | Backlog home and label config |
| `dor_criteria` | string[] | yes | [] | Definition of Ready checklist |
| `voc_sources` | VocSources | yes | — | Available customer-signal sources |
| `prioritization_weights` | Weights | yes | — | Scoring weights 1-5 |
| `cross_os` | CrossOS | yes | — | Sibling plugin availability flags |
| `created_at` | string | no | — | ISO date |
| `updated_at` | string | no | — | ISO date |

### Persona Object

| Field | Type | Description |
|---|---|---|
| `name` | string | Short label |
| `description` | string | 1-2 sentences |
| `pains` | string[] | Top 3-5 pain points |
| `alternatives` | string[] | What they use today |
| `buying_trigger` | string | Event that starts the buying process |

### Competitor Object

| Field | Type | Description |
|---|---|---|
| `name` | string | Competitor name |
| `type` | string | `direct` or `adjacent` |
| `url` | string | Website URL |
| `review_pages` | object | `g2`, `capterra`, `trustpilot` URLs |
| `differentiators` | string[] | Known diffs vs. us (optional) |

### Backlog Object

| Field | Type | Default | Description |
|---|---|---|---|
| `location` | string | `github` | `github`, `linear`, `jira`, `local` |
| `repo` | string | — | `owner/repo` for GitHub |
| `default_labels` | object | — | Category / status / priority label maps |

### VocSources Object

| Field | Type | Default | Description |
|---|---|---|---|
| `support_email` | object | `{enabled: false}` | `enabled`, `address`, `log_path` |
| `stripe_churn` | object | `{enabled: false}` | `enabled`, `collection_method` |
| `competitor_reviews` | object | `{enabled: false}` | `enabled`, `sites: []` (array of `{competitor, site, url}`) |
| `forums` | object | `{enabled: false}` | `enabled`, `subs: []` array of forum/sub URLs |
| `analytics` | object | `{enabled: false}` | `enabled`, `platform`, `dashboard_url` |
| `prospect_interviews` | object | `{enabled: false}` | `enabled`, `cadence`, `log_path` |
| `nps_csat` | object | `{enabled: false}` | `enabled`, `tool`, `dashboard_url` |

### Prioritization Weights Object

All weights are integers 1-5 (higher = more influence on score).

| Field | Default | Description |
|---|---|---|
| `regulatory_urgency` | 5 | Pressure from deadlines, enforcement |
| `customer_pain` | 5 | VoC signal strength |
| `strategic_fit` | 3 | Roadmap alignment, positioning |
| `effort_inverse` | 3 | Smaller items weigh higher |
| `revenue_impact` | 4 | Likely conversion/retention effect |

**Priority score formula** (used by `lead-po`):

```
score =
  regulatory_urgency * urgency_signal (0-1)
+ customer_pain      * pain_signal    (0-1)
+ strategic_fit      * fit_signal     (0-1)
+ effort_inverse     * (1 - effort_normalized) (0-1)
+ revenue_impact     * revenue_signal (0-1)

P0: score >= 0.80 * max_score
P1: score >= 0.60 * max_score
P2: score >= 0.40 * max_score
P3: score <  0.40 * max_score
```

### CrossOS Object

| Field | Type | Default | Description |
|---|---|---|---|
| `legal_os` | boolean | false | Whether `legal-os` plugin is installed |
| `marketing_os` | boolean | false | Whether `marketing-os` plugin is installed |

## Example Profile

```json
{
  "active_profile": "aurora",
  "profiles": {
    "aurora": {
      "product_name": "Aurora",
      "pitch": "Compliance-tracking SaaS for European SMEs by Acme Studio",
      "domain": "aurora.example.com",
      "stage": "early-access",
      "repo": "acme-studio/aurora",
      "personas": [
        {
          "name": "Italian SME Founder",
          "description": "1-50 employee company founder, non-lawyer, dealing with Garante requirements and cookie compliance for the first time",
          "pains": [
            "Cookie banner confusion",
            "Privacy policy drafting without a lawyer",
            "Fear of Garante fines"
          ],
          "alternatives": ["Word templates", "Consent-widget tools", "Lawyer on retainer"],
          "buying_trigger": "Received first customer data request or Garante letter"
        }
      ],
      "markets": {
        "primary": ["IT"],
        "secondary": ["DE", "FR", "ES", "GB"]
      },
      "covered_frameworks": ["gdpr", "eprivacy"],
      "local_frameworks": ["uk_gdpr"],
      "competitors": [
        {
          "name": "ExampleConsent",
          "type": "direct",
          "url": "https://exampleconsent.example.com",
          "review_pages": {
            "g2": "https://www.g2.com/products/exampleconsent/reviews",
            "capterra": "",
            "trustpilot": ""
          },
          "differentiators": ["They're broader geo", "We're cheaper for Italy"]
        }
      ],
      "status_quo_alternative": "Word templates + lawyer on retainer",
      "backlog": {
        "location": "github",
        "repo": "acme-studio/aurora",
        "default_labels": {
          "category": {
            "regulatory": "po:regulatory",
            "localization": "po:localization",
            "voc": "po:voc"
          },
          "status": {
            "ready": "ready",
            "needs_clarification": "needs-clarification"
          },
          "priority": {
            "p0": "p0",
            "p1": "p1",
            "p2": "p2",
            "p3": "p3"
          }
        }
      },
      "dor_criteria": [
        "User story in 'As a … I want … so that …' form",
        "At least 2 acceptance criteria",
        "Source signal cited",
        "Risks and assumptions listed",
        "T-shirt size estimate",
        "No unresolved open questions"
      ],
      "voc_sources": {
        "support_email": {"enabled": true, "address": "support@aurora.example.com", "log_path": ""},
        "stripe_churn": {"enabled": false, "collection_method": ""},
        "competitor_reviews": {"enabled": true, "sites": []},
        "forums": {"enabled": true, "subs": ["r/gdpr", "r/privacy"]},
        "analytics": {"enabled": false, "platform": "", "dashboard_url": ""},
        "prospect_interviews": {"enabled": false, "cadence": "", "log_path": ""},
        "nps_csat": {"enabled": false, "tool": "", "dashboard_url": ""}
      },
      "prioritization_weights": {
        "regulatory_urgency": 5,
        "customer_pain": 5,
        "strategic_fit": 3,
        "effort_inverse": 3,
        "revenue_impact": 4
      },
      "cross_os": {
        "legal_os": true,
        "marketing_os": true
      },
      "created_at": "2026-04-20",
      "updated_at": "2026-04-20"
    }
  }
}
```

## Framework Codes

Same codes as legal-os (intentional — enables cross-OS framework matching):

| Code | Full Name |
|---|---|
| `gdpr` | General Data Protection Regulation |
| `eprivacy` | ePrivacy / Cookie Directive |
| `csrd` | Corporate Sustainability Reporting Directive |
| `esrs` | European Sustainability Reporting Standards |
| `ai_act` | EU AI Act |
| `nis2` | Network and Information Security Directive 2 |
| `dora` | Digital Operational Resilience Act |
| `data_act` | Data Act |
| `uk_gdpr` | UK GDPR |
| `nfadp` | Swiss new Federal Act on Data Protection |
| `kvkk` | Turkish Personal Data Protection Law |
