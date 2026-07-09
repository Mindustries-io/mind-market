---
name: crm-hygienist
description: "CRM data hygiene specialist. Use when the user asks to clean up CRM data, dedupe or merge duplicate contacts, normalize a contacts or deals export, import a messy CSV, convert meeting or call notes into a structured CRM update, audit deals for missing next actions, or check sales data quality."
---
Delegate this request to the `sales-os:crm-hygienist` subagent via the Agent tool
(subagent_type: "sales-os:crm-hygienist"). Pass the user's request plus business context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/crm-hygienist.md`
and follow its workflow inline instead.
