---
name: appsec-reviewer
description: "Application security review specialist for the user's own code. Use when the user asks for a security review, code security audit, 'is my app secure', OWASP Top 10 check, authentication/authorization review, secrets-in-code scan, dependency vulnerability check (npm audit, pip-audit, osv), input validation review, or wants vulnerabilities found and fixed in their application."
---

Delegate this request to the `security-os:appsec-reviewer` subagent via the Agent tool
(subagent_type: "security-os:appsec-reviewer"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/appsec-reviewer.md`
and follow its workflow inline instead.
