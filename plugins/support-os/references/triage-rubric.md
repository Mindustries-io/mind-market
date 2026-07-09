# Triage Rubric

## Severity

| Level | Definition | Examples | First-response target |
|---|---|---|---|
| **S1** | Product unusable, data at risk, or security issue — for one or many customers | Outage, data loss, "I can't log in at all", vulnerability report, payment charged twice | ASAP, jumps every queue |
| **S2** | Core feature broken or blocked, workaround painful or none | Export fails, integration down, blocked mid-onboarding | Same day |
| **S3** | Degraded/annoying but working; billing questions; refund requests | Slow page, confusing UI, invoice question, cancel/refund ask | Within SLA `first_response_hours` |
| **S4** | Questions, how-tos, feature requests, feedback, praise | "How do I…", "Would be nice if…", thank-you notes | Within SLA, batchable |

Tie-break upward: if in doubt between two levels, pick the higher one when the requester is a paying customer or the ticket mentions money, security, or legal.

## Topic taxonomy (one primary tag per ticket)

`billing` · `refund` · `bug` · `how-to` · `feature-request` · `account` (login, password, deletion, data requests) · `integration` · `other`

Notes: a data-deletion or data-export request is `account` + severity S2 minimum (legal deadlines may apply — flag it). A refund demand inside an angry bug report is `refund` (money wins the primary tag), with the bug noted for `bug-report` routing too.

## Sentiment

`positive` · `neutral` · `frustrated` (annoyed but cooperative) · `angry` (hostile language, threats to leave/post publicly, ALL CAPS, legal threats)

## Churn-risk flags (any one triggers the flag)

- Cancellation vocabulary: "cancel", "switching", "not renewing", "downgrade"
- Competitor named as an alternative
- Repeat contact (2+) on the same unresolved issue
- Paying customer + `angry` sentiment
- Refund demand citing missing value ("this doesn't do what I paid for")

## Routing decisions

| Route | When | Handoff |
|---|---|---|
| `answer-now` | Needs a written reply; answerable from product knowledge/config | `response-drafter` |
| `kb-link` | An existing article answers it, or the question recurs and deserves one | Existing: cite the article. Gap: list for `kb-writer` |
| `bug-report` | Reproducible defect | Repro summary for the user's issue tracker; customer still gets an `answer-now` acknowledgment |
| `user-decision` | Refunds above `REFUND_POLICY.user_decides_above`, legal threats, security reports, press/partnership, anything in `ESCALATION.user_decides` | The user, with a one-line recommendation |

A ticket can carry a secondary route (e.g. `bug-report` + `answer-now` ack) — list the primary first.

## Priority ordering within the queue

1. S1 (always first)
2. SLA breach risk: age ≥ 80% of `first_response_hours` and no reply yet
3. Churn-risk flagged
4. Severity (S2 → S4), then oldest first

## CSV parsing hints

| Tool | Typical columns |
|---|---|
| Zendesk | `Subject`, `Description`, `Requester`, `Created at`, `Priority`, `Status` |
| Help Scout | `Subject`, `Preview`/`Body`, `Customer Email`, `Created At`, `Status` |
| Freshdesk | `Subject`, `Description`, `Requester Email`, `Created Time`, `Priority` |
| Intercom | `Conversation subject`/first message, `Contact`, `Started at` |
| Plain inbox | `From`, `Subject`, `Date`, `Body` |

If columns don't match, map by best guess, state the mapping, and ask only if the body column is genuinely ambiguous. Ignore the tool's own priority column for severity (recompute with this rubric) but report it if present.
