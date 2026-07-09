# CISO Output Formats

Templates for multi-agent syntheses. Single-agent (FAST MODE) responses use the specialist's own output format — do not rewrap them.

## Security Health Report (full pipeline)

```markdown
# Security Health Report — {ORG} — {date}

## Executive Summary
- Overall grade: {A–F} ({trend vs last report, if memory has one})
- 3-5 bullets: the state of security in plain language

## Scorecard
| Domain | Score | Grade | Worst gap |
|---|---|---|---|
| Posture (accounts/cloud/backups/DNS/endpoint/secrets) | …% | … | … |
| Application security | … findings (C/H/M/L) | … | … |
| Design / threat exposure | top threat rating | … | … |
| Third-party / vendors | … vendors, highest risk | … | … |

## Top 5 Risks
| # | Risk | Severity | Source agent | Fix | Effort |
|---|---|---|---|---|---|

## Action Plan
**This week:** …
**This month:** …
**This quarter:** …

## Coverage & Caveats
What was reviewed, what wasn't, tooling gaps. AI-assisted review — not a
penetration test or certification (see SAFETY.md).
```

## Incident Status Update (relayed from security-incident)

```markdown
## Incident {slug} — {SEV1|SEV2|SEV3} — {phase: triage/contain/scope/eradicate/recover}
- What happened: …
- Contained: {yes/no — what was done}
- Personal data affected: {yes/possibly/no} → {legal-os handoff status or notification reminder}
- Next 3 actions: 1… 2… 3…
```

## Memory conventions

| Key pattern | Written by | Content |
|---|---|---|
| `sec: {ORG} posture {date}` | hardening-auditor | Scorecard + top actions |
| `sec: {ORG} appsec {date}` | appsec-reviewer | CRITICAL/HIGH findings summary |
| `sec: {ORG} threats {feature}` | threat-modeler | Top 3 threats + decisions |
| `sec:vendor:<vendor-slug>` | vendor-security | Rating, access scope, attestations, breach history (shared with legal-os:vendor-risk) |
| `sec:incident:<date-slug>` | security-incident | Summary, personal-data determination, notification status (shared with legal-os:incident-response) |
