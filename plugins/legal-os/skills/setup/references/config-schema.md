# Legal OS Configuration Schema

## File Location
`~/.claude/plugins/data/legal-os/config.json`

## Schema

| Field | Type | Required | Description |
|---|---|---|---|
| `active_profile` | string | yes | Slug of the currently active organization profile |
| `profiles` | object | yes | Map of profile slug -> profile object |

### Profile Object

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `org_name` | string | yes | - | Legal entity name |
| `org_type` | string | yes | "sme" | sme, startup, mid-market, enterprise |
| `employee_count` | string | yes | - | Range: "1-10", "11-50", "51-250", "251-1000", "1000+" |
| `country` | string | yes | - | 2-letter ISO country code of incorporation |
| `industry` | string | yes | - | Industry vertical |
| `domain` | string | no | - | Primary website domain |
| `jurisdictions` | string[] | yes | [country] | Countries where org processes data |
| `active_frameworks` | string[] | yes | ["gdpr"] | Compliance frameworks in scope |
| `dpo` | object | no | - | DPO configuration (see below) |
| `supervisory_authority` | object | no | - | Primary DPA (see below) |
| `obligoboard` | object | no | - | ObligoBoard integration (see below) |
| `contract_defaults` | object | no | - | Contract negotiation playbook (see below) |
| `risk_appetite` | object | no | - | Risk thresholds (see below) |
| `reporting` | object | no | - | Reporting preferences (see below) |
| `created_at` | string | no | - | ISO date of profile creation |
| `updated_at` | string | no | - | ISO date of last update |

### DPO Object

| Field | Type | Default | Description |
|---|---|---|---|
| `appointed` | boolean | false | Whether a DPO is appointed |
| `name` | string | "" | DPO full name |
| `email` | string | "" | DPO contact email |
| `type` | string | "internal" | "internal" or "external" |
| `dpo_required` | string | "unsure" | "yes", "no", or "unsure" |

### Supervisory Authority Object

| Field | Type | Default | Description |
|---|---|---|---|
| `name` | string | "" | DPA short name (e.g., "BfDI", "CNIL", "ICO") |
| `full_name` | string | "" | Full authority name |
| `country` | string | "" | 2-letter ISO code |
| `notification_url` | string | "" | Breach notification portal URL |
| `general_contact` | string | "" | General contact URL or email |

### ObligoBoard Integration Object

| Field | Type | Default | Description |
|---|---|---|---|
| `base_url` | string | "" | ObligoBoard instance URL |
| `api_available` | boolean | false | Whether API token auth is available |
| `api_key` | string | "" | API key (Phase 2, when available) |
| `manual_sync` | boolean | true | Using snapshot-based sync |

### Contract Defaults Object

| Field | Type | Default | Description |
|---|---|---|---|
| `governing_law` | string | "" | Default governing law |
| `dispute_resolution` | string | "" | Default dispute resolution mechanism |
| `standard_terms` | string | "negotiate" | "own", "counterparty", "negotiate" |
| `liability_cap` | string | "" | Acceptable max liability description |
| `unacceptable_liability` | string | "" | Hard limits on liability |
| `required_clauses` | string[] | [] | Clauses that must appear in all contracts |
| `unacceptable_clauses` | string[] | [] | Clauses to always flag and reject |

### Risk Appetite Object

| Field | Type | Default | Description |
|---|---|---|---|
| `level` | string | "moderate" | "conservative", "moderate", "aggressive" |
| `escalation_threshold` | string | "medium" | Risk level requiring human review: "low", "medium", "high" |
| `auto_approve_threshold` | string | "none" | Risk level agents can handle alone: "none", "low" |

### Reporting Object

| Field | Type | Default | Description |
|---|---|---|---|
| `compliance_report_day` | string | "monday" | Day for weekly compliance summary |
| `regulatory_scan_frequency` | string | "weekly" | "weekly", "biweekly", "monthly" |
| `board_report_frequency` | string | "quarterly" | "monthly", "quarterly" |

## Common Country Codes (EU/EEA + UK)

| Code | Country | Primary DPA |
|---|---|---|
| AT | Austria | DSB |
| BE | Belgium | APD/GBA |
| BG | Bulgaria | CPDP |
| HR | Croatia | AZOP |
| CY | Cyprus | Commissioner |
| CZ | Czech Republic | UOOU |
| DK | Denmark | Datatilsynet |
| EE | Estonia | AKI |
| FI | Finland | Tietosuojavaltuutettu |
| FR | France | CNIL |
| DE | Germany | BfDI (federal) + 16 state DPAs |
| GR | Greece | HDPA |
| HU | Hungary | NAIH |
| IE | Ireland | DPC |
| IT | Italy | Garante |
| LV | Latvia | DVI |
| LT | Lithuania | VDAI |
| LU | Luxembourg | CNPD |
| MT | Malta | IDPC |
| NL | Netherlands | AP |
| PL | Poland | UODO |
| PT | Portugal | CNPD |
| RO | Romania | ANSPDCP |
| SK | Slovakia | UOOU |
| SI | Slovenia | IP RS |
| ES | Spain | AEPD |
| SE | Sweden | IMY |
| GB | United Kingdom | ICO |
| NO | Norway | Datatilsynet |
| IS | Iceland | Personuvernd |
| LI | Liechtenstein | DSS |
| CH | Switzerland | FDPIC |

## Compliance Framework Codes

| Code | Full Name | Key Regulation |
|---|---|---|
| `gdpr` | General Data Protection Regulation | Regulation (EU) 2016/679 |
| `data_processor` | Data Processor Obligations | GDPR Articles 28-31 |
| `nis2` | Network and Information Security Directive | Directive (EU) 2022/2555 |
| `ai_act` | EU Artificial Intelligence Act | Regulation (EU) 2024/1689 |
| `dora` | Digital Operational Resilience Act | Regulation (EU) 2022/2554 |
| `csrd` | Corporate Sustainability Reporting | Directive (EU) 2022/2464 |
| `eprivacy` | ePrivacy / Cookie Directive | Directive 2002/58/EC |
