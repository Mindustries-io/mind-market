# Support Lead — Synthesis Output Formats

Used only for multi-agent runs. FAST MODE relays the specialist's own output untouched.

## Inbox Session Report (triage → drafter pipeline)

```markdown
# Inbox session — {PRODUCT} — {date}
**{n} tickets** ({date range}) · {x} drafted · {y} KB-linked · {z} need your decision

## Needs your decision ({z})
| # | Requester | Issue | Why you | Recommendation |
|---|---|---|---|---|

## Ready to send ({x})
### Ticket #{id} — {summary}
> (severity, sentiment, churn flag)
```text
{ready-to-send draft}
```

## KB gaps spotted
- {question} — asked {n}× → candidate article "{title}"

## Next action
{single sentence}
```

## Support Health Report (collaborative runs / monthly review)

```markdown
# Support health — {PRODUCT} — {period}
## Executive summary
- 3-5 bullets: volume, top pattern, churn risk, SLA status

## Queue status          (from ticket-triage)
## Trends & signals      (from insights-analyst — pattern table with n + strength)
## Churn watchlist       (anonymized requester IDs + evidence)
## Documentation queue   (from kb-writer — deflection-ranked)
## Product signal        (Discovery Insights if po-os enabled, else plain findings)
## Decisions needed
## Next actions (max 3)
```

## Rules

- User decisions always come first — never below the fold.
- Every trend claim carries `n=` and a date range.
- Drafts appear in fenced blocks, ready to copy verbatim.
- Anonymize requesters everywhere except inside a specific draft reply.
