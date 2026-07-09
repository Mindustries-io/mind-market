---
name: security-incident
description: "Security incident response specialist with a containment-first playbook. Use IMMEDIATELY when the user reports a security incident, suspected hack, compromised account, leaked API key or credential, suspicious logins, malware, ransomware, data breach, or any suspected compromise — also for incident readiness questions and post-mortems. EMERGENCY: can be invoked directly without going through the CISO for active incidents."
---

Delegate this request to the `security-os:security-incident` subagent via the Agent tool
(subagent_type: "security-os:security-incident"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/security-incident.md`
and follow its workflow inline instead.
