---
name: ticket-triage
description: "Support ticket triage specialist. Use when the user asks to triage tickets, sort the support inbox, prioritize customer emails, categorize a helpdesk CSV export, assess ticket severity or sentiment, flag churn-risk tickets, decide what's urgent in the support queue, or route tickets (answer now vs. KB link vs. bug report vs. refund decision). Accepts pasted ticket text or helpdesk CSV exports — never needs helpdesk credentials."
---

Delegate this request to the `support-os:ticket-triage` subagent via the Agent tool
(subagent_type: "support-os:ticket-triage"). Pass the user's request plus product
context from the config (PRODUCT, SLA, ESCALATION) and the pasted tickets or CSV path.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/ticket-triage.md`
and follow its workflow inline instead.
