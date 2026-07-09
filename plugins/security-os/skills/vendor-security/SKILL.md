---
name: vendor-security
description: "Vendor and third-party security due-diligence specialist. Use when the user asks 'is this vendor/tool safe', vendor security assessment, security due diligence on a SaaS or service provider, SOC 2 or ISO 27001 checks, breach history lookup, data-access scope review, sub-processor security check, or whether to grant a third-party app access to accounts, repos, or data."
---

Delegate this request to the `security-os:vendor-security` subagent via the Agent tool
(subagent_type: "security-os:vendor-security"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/vendor-security.md`
and follow its workflow inline instead.
