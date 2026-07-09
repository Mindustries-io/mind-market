# STRIDE Threat Model Template

Prompts and table template behind the `threat-modeler` agent. Keep the model solopreneur-sized: one session, one table, decisions attached.

## Framing (fill first)

```markdown
**System:** <one paragraph — what it does, main data flow client → API → store → third parties>
**Assets:** <from crown_jewels + anything feature-specific>
**Actors:** <realistic: opportunistic scanners, credential stuffers, malicious users,
compromised dependency/vendor, founder's own mistake>
**Trust boundaries:** <every privilege/ownership line data crosses>
**Assumptions / out of scope:** <state them — they bound the model's validity>
```

## STRIDE prompts per boundary

| Letter | Question to ask at each element/boundary | Solopreneur-typical examples |
|---|---|---|
| **S**poofing | Can someone pretend to be a user, the service, or a webhook sender? | Credential stuffing (reused passwords); unsigned webhook accepted as Stripe; phishing the founder's email |
| **T**ampering | Can data or code be modified in transit, at rest, or in the pipeline? | Client-side price/role fields trusted; unpinned CI action; direct DB edit via exposed port |
| **R**epudiation | Could you prove what happened after the fact? | No audit log on deletes/exports; shared admin account; logs on the same box that got compromised |
| **I**nformation disclosure | Where could data leak to someone who shouldn't see it? | IDOR across tenants; verbose errors; public bucket; PII in logs or analytics; AI tool fed customer data |
| **D**enial of service | What makes the service or the founder unavailable? | Unbounded uploads/queries; no rate limit on expensive endpoints; billing-DoS (cost spike); single laptop with no backup |
| **E**levation of privilege | Can a low-privilege actor gain higher privilege? | Missing server-side role check; mass assignment sets `is_admin`; scoped key that can mint broader keys |

Skip a category only with a one-line justification (e.g. "DoS: static site on CDN — accepted").

## Rating

- **Likelihood** — Low / Medium / High. Calibration: anything mass-scanned or automated (exposed panels, known CVEs, credential stuffing) is High even for a tiny target; attacks needing targeted human effort against a solo business are usually Low-Medium.
- **Impact** — Low / Medium / High, measured against the crown jewels (data loss, money, downtime, trust).
- Priority order: H/H → H/M or M/H → M/M → rest.

## Threat table template

| # | Asset | Threat (STRIDE) | Scenario (one sentence) | Likelihood | Impact | Mitigation | Decision / Owner |
|---|---|---|---|---|---|---|---|
| 1 | | | | | | | Mitigate / Accept / Transfer / Investigate |

Decisions:
- **Mitigate** — with a concrete, one-person-sized action (prefer platform defaults and managed services over custom security code).
- **Accept** — with rationale written down; revisit date optional.
- **Transfer** — insurance or pushing the risk to a vendor contractually.
- **Investigate** — unknowns blocking a rating; becomes a task.

## Wrap-up

- **Top 3 threats** with the single next action each.
- Store to memory: `sec: {ORG} threats {feature}` — top threats + decisions, so future reviews check whether mitigations landed.
- Suggest revisiting the model when the feature's data flow or trust boundaries change, not on a calendar.
