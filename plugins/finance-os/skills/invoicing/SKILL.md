---
name: invoicing
description: "Invoicing and accounts-receivable specialist. Use when the user asks to draft an invoice, create an invoice, bill a client, chase a late payment, write a payment reminder, handle an overdue or unpaid invoice, review outstanding invoices, check who owes money, mark an invoice paid, or manage the invoice ledger."
---

Delegate this request to the `finance-os:invoicing` subagent via the Agent tool
(subagent_type: "finance-os:invoicing"). Pass the user's request plus business
details, payment terms, and VAT setting from the config.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/invoicing.md`
and follow its workflow inline instead.
