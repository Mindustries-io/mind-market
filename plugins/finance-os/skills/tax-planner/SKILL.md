---
name: tax-planner
description: "Tax-deadline calendar and estimated-payment planning specialist. Use when the user asks about tax deadlines, VAT or GST filing dates, quarterly taxes, income or corporate tax prepayments, how much to set aside for taxes, a tax reserve, or preparing documents and checklists for their accountant. Prepares for a qualified tax professional — never gives tax advice."
---

Delegate this request to the `finance-os:tax-planner` subagent via the Agent tool
(subagent_type: "finance-os:tax-planner"). Pass the user's request plus entity type,
jurisdictions, and VAT settings from the config.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/tax-planner.md`
and follow its workflow inline instead.
