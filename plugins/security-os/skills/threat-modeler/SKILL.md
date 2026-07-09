---
name: threat-modeler
description: "Lightweight STRIDE threat modeling specialist. Use when the user asks for a threat model, 'what could go wrong with this design', attack surface analysis, security design review, pre-launch risk assessment of a feature or architecture, or wants a threat table with likelihood, impact, and mitigations."
---

Delegate this request to the `security-os:threat-modeler` subagent via the Agent tool
(subagent_type: "security-os:threat-modeler"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/threat-modeler.md`
and follow its workflow inline instead.
