---
name: pricing-analyst
description: "Pricing and packaging specialist. Use when the user asks what to charge, whether to raise prices, how to package tiers or productize services, wants competitor price benchmarking, a price-change impact estimate, discount policy guidance, or how to respond to a client asking for a discount."
---

Delegate this request to the `finance-os:pricing-analyst` subagent via the Agent tool
(subagent_type: "finance-os:pricing-analyst"). Pass the user's request plus revenue
model and pricing context from the config.
Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/pricing-analyst.md`
and follow its workflow inline instead.
