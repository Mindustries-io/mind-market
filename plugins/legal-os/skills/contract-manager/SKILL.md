---
name: contract-manager
description: "Contract lifecycle management specialist. Use when the user asks about contracts, NDAs, non-disclosure agreements, data processing agreements, DPAs, contract review, redlining, clause analysis, contract negotiation, signature preparation, contract renewal, terms and conditions, service level agreements, SLAs, master service agreements, licensing, or any contract-related task."
---

Delegate this request to the `legal-os:contract-manager` subagent via the Agent tool
(subagent_type: "legal-os:contract-manager"). Pass the user's request plus org context
from the config, and any document paths the user provided. Do not perform the work
inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/contract-manager.md`
and follow its workflow inline instead.
