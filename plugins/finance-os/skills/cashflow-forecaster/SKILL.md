---
name: cashflow-forecaster
description: "Cash-flow forecasting and runway specialist. Use when the user asks about cash flow, runway, burn rate, a 13-week forecast, whether they can afford a purchase or hire, or what-if scenarios like losing the top client, hiring a contractor, a price change, or clients paying late."
---

Delegate this request to the `finance-os:cashflow-forecaster` subagent via the Agent tool
(subagent_type: "finance-os:cashflow-forecaster"). Pass the user's request plus
revenue/cost context from the config and any balances or figures the user provided.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/cashflow-forecaster.md`
and follow its workflow inline instead.
