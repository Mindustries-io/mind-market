# Contract Redline Rules — Negotiation Playbook

## How to Use This Playbook

When reviewing a counterparty draft, compare each clause against:
1. The **organization's config** (`contract_defaults` in config.json)
2. The **clause library** (clause-library.md) for standard/red flag patterns
3. This **redline rules** document for specific negotiation positions

For each deviation, generate a redline with:
- The original clause (quoted)
- The issue identified
- Proposed alternative language
- Risk level if accepted as-is
- Whether it's a deal-breaker or negotiation point

---

## Priority Framework

### P1 — Deal-Breakers (must be changed)
These are non-negotiable. If the counterparty refuses, escalate to human review:
- Unlimited liability for the org
- No data protection provisions despite personal data processing
- Governing law/jurisdiction that fundamentally disadvantages the org
- IP assignment of the org's pre-existing intellectual property
- Clauses that violate mandatory law (GDPR, employment law, etc.)
- Non-compete clauses that restrict the org's core business

### P2 — Strong Position (push hard)
These should be changed but acceptance is possible with senior approval:
- One-sided indemnification
- Liability cap below 6 months
- No termination for convenience
- Sub-processor engagement without prior authorization
- Auto-renewal with short notice period
- Breach notification >48 hours

### P3 — Preferred Position (negotiate)
These are preferences, not requirements:
- Governing law preference (accept EU alternatives)
- Liability cap range (accept 6-24 months)
- Termination notice period (accept 30-90 days)
- Confidentiality duration (accept 2-5 years)
- Service credit percentages
- Force majeure scope details

### P4 — Accept (minor issues)
These are cosmetic or low-impact:
- Formatting and structure preferences
- Defined term naming conventions
- Notice address formatting
- Exhibit numbering
- Minor definitional differences

---

## DPA-Specific Redline Rules (GDPR Article 28)

### Mandatory DPA Elements Checklist

Every DPA must contain ALL of the following. If missing, add via redline:

| # | Requirement | Art. 28(3) ref | Redline if missing |
|---|---|---|---|
| 1 | Subject matter and duration | (a) | Add description of processing scope |
| 2 | Nature and purpose | (a) | Add processing purpose statement |
| 3 | Types of personal data | (a) | Add data category list |
| 4 | Categories of data subjects | (a) | Add data subject category list |
| 5 | Process only on documented instructions | (a) | Add instruction obligation clause |
| 6 | Confidentiality of personnel | (b) | Add confidentiality requirement |
| 7 | Security measures (Art. 32) | (c) | Add security measures clause |
| 8 | Sub-processor conditions | (d) | Add prior written authorization requirement |
| 9 | Assist with data subject rights | (e) | Add assistance obligation |
| 10 | Assist with security, breach, DPIA | (f) | Add assistance for Art. 32-36 |
| 11 | Delete or return data at end | (g) | Add end-of-contract data handling |
| 12 | Audit and inspection rights | (h) | Add audit right clause |
| 13 | Inform if instruction infringes GDPR | - | Add notification obligation |

### DPA Redline Positions

| Issue | Org Position | Fallback |
|---|---|---|
| **Sub-processor approval** | Prior specific written authorization | Prior general authorization with right to object within 30 days |
| **Breach notification timing** | Within 24 hours | Within 48 hours (never accept >72h) |
| **Audit rights** | On-site audit with 30 days notice | Annual third-party audit report (SOC 2/ISO 27001) + on-site for cause |
| **Data location** | EEA only | EEA + adequate countries; never accept US-only without safeguards |
| **Data deletion at termination** | Delete within 30 days, certification | Delete within 90 days, confirmation |
| **Sub-processor liability** | Processor remains fully liable | Processor liable, reasonable cooperation with sub-processor claims |

---

## Redline Response Templates

### For P1 (Deal-Breaker) Issues:

```
REDLINE: Section [X] — [Clause Title]

ORIGINAL: "[quote the problematic clause]"

ISSUE: [Description of why this is unacceptable]
RISK: CRITICAL — This clause [specific risk: violates GDPR / creates unlimited exposure / etc.]

PROPOSED: "[alternative language]"

NOTE: This is a required change. We cannot proceed without modification of this clause.
```

### For P2-P3 (Negotiation) Issues:

```
REDLINE: Section [X] — [Clause Title]

ORIGINAL: "[quote the clause]"

ISSUE: [Description of the concern]
RISK: {HIGH / MEDIUM} — [Specific risk description]

PROPOSED: "[preferred alternative language]"

FALLBACK: "[minimum acceptable alternative]"

NOTE: Our standard position is [X]. We are willing to discuss alternatives that [Y].
```

---

## Counterparty Negotiation Signals

### Signs of a reasonable counterparty:
- Accepts mutual obligations (not one-sided)
- Willing to discuss liability caps
- Provides substantive DPA
- Responds to redlines with counter-proposals
- Accepts standard market positions

### Signs requiring escalation:
- "These are our standard terms, non-negotiable"
- Refuses to add DPA despite processing personal data
- Insists on unlimited liability for the org
- Won't provide sub-processor list or audit rights
- Demands IP assignment beyond the scope of the engagement
- Won't identify data processing locations

When encountering resistance, note it in the review output and recommend escalation to human legal counsel with specific talking points.
