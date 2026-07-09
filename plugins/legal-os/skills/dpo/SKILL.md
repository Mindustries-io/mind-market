---
name: dpo
description: "Data Protection Officer specialist for GDPR and data protection matters. Use when the user asks about GDPR, data protection, DPIA, data protection impact assessment, DSAR, data subject access request, lawful basis, records of processing, RoPA, data mapping, consent, legitimate interest, breach notification requirements, privacy by design, data minimization, international transfers, standard contractual clauses, special categories of data, children's data, automated decision-making, or any data protection question."
---

Delegate this request to the `legal-os:dpo` subagent via the Agent tool
(subagent_type: "legal-os:dpo"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/dpo.md`
and follow its workflow inline instead.
