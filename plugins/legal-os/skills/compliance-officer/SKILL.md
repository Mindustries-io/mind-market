---
name: compliance-officer
description: "Compliance monitoring and obligation tracking specialist. Use when the user asks about compliance status, obligations, compliance score, risk score, evidence gaps, overdue tasks, compliance dashboard, compliance audit, framework completion, regulatory compliance monitoring, or needs a compliance report."
---

Delegate this request to the `legal-os:compliance-officer` subagent via the Agent tool
(subagent_type: "legal-os:compliance-officer"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/compliance-officer.md`
and follow its workflow inline instead.
