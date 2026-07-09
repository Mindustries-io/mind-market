---
name: vendor-risk
description: "Vendor and third-party risk assessment specialist. Use when the user asks about vendor risk assessment, vendor onboarding, vendor due diligence, data processing agreements (DPAs), sub-processor management, third-party compliance, vendor audit, processor obligations, supply chain risk, cloud provider assessment, SaaS vendor compliance, or any vendor/third-party related legal or compliance question."
---

Delegate this request to the `legal-os:vendor-risk` subagent via the Agent tool
(subagent_type: "legal-os:vendor-risk"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/vendor-risk.md`
and follow its workflow inline instead.
