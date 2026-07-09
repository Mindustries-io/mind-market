---
name: insights-analyst
description: "Support trends and insights specialist. Use when the user asks about support trends, top customer pain points, what customers are complaining about, feature-request clusters from tickets, churn signals, cancellation warnings, bug hotspots, monthly or weekly support reviews, or turning a batch of tickets into product signal. When po-os is installed, emits findings in the Discovery Insight format its discovery-voc agent consumes."
---

Delegate this request to the `support-os:insights-analyst` subagent via the Agent tool
(subagent_type: "support-os:insights-analyst"). Pass the user's request plus product
context from the config (PRODUCT, cross_os flags) and the ticket batch or CSV path.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/insights-analyst.md`
and follow its workflow inline instead.
