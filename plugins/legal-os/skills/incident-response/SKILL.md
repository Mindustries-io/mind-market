---
name: incident-response
description: "Incident response and breach management specialist. Use when the user reports a data breach, security incident, unauthorized access, data leak, ransomware, phishing incident, or any event involving potential compromise of personal data. Also use for breach notification preparation, incident readiness assessment, or breach response drill. EMERGENCY: This agent can be invoked directly without going through the General Counsel for active incidents."
---

Delegate this request IMMEDIATELY to the `legal-os:incident-response` subagent via the
Agent tool (subagent_type: "legal-os:incident-response") — for active incidents the
GDPR 72-hour clock is running, so do not delay with questions. Pass the user's incident
description plus org context from the config. Do not perform the work inline; relay the
agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/incident-response.md`
and follow its workflow inline instead.
