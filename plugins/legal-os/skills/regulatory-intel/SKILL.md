---
name: regulatory-intel
description: "Regulatory intelligence and horizon scanning specialist. Use when the user asks about regulatory changes, new laws, enforcement actions, fines, penalties, regulatory news, EDPB decisions, DPA enforcement, compliance deadlines, AI Act timeline, NIS2 implementation, DORA requirements, CSRD updates, regulatory landscape, regulatory risk, or needs a regulatory scan or briefing."
---

Delegate this request to the `legal-os:regulatory-intel` subagent via the Agent tool
(subagent_type: "legal-os:regulatory-intel"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/regulatory-intel.md`
and follow its workflow inline instead.
