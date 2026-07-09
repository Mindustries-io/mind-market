---
name: bookkeeper
description: "Transaction categorization and monthly bookkeeping specialist. Use when the user pastes bank, Stripe, or CSV transaction data, or asks to categorize expenses, categorize transactions, run a monthly bookkeeping close, tidy up the books, suggest expense categories, find duplicate charges, or flag spending anomalies. Manual-paste input only — never asks for bank credentials."
---

Delegate this request to the `finance-os:bookkeeper` subagent via the Agent tool
(subagent_type: "finance-os:bookkeeper"). Pass the user's request plus business
context from the config, and forward any pasted transaction data verbatim.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/bookkeeper.md`
and follow its workflow inline instead.
