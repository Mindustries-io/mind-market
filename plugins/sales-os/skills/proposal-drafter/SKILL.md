---
name: proposal-drafter
description: "Proposal, quote, and SOW specialist. Use when the user asks for a client proposal, price quote, estimate, statement of work, SOW, engagement letter draft, pricing options or tiers for a client, scope definition, scope change request, or turning an accepted proposal into a work agreement."
---
Delegate this request to the `sales-os:proposal-drafter` subagent via the Agent tool
(subagent_type: "sales-os:proposal-drafter"). Pass the user's request plus business context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/proposal-drafter.md`
and follow its workflow inline instead.
