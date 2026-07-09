---
name: pipeline-manager
description: "Pipeline management specialist. Use when the user asks about their sales pipeline, deal stages, moving a deal, stale deals, deal next actions, weekly pipeline review, pipeline value, forecast snapshot, win/loss log, logging a won or lost deal, or importing deals from a CRM export (HubSpot, Pipedrive, Attio, CSV)."
---
Delegate this request to the `sales-os:pipeline-manager` subagent via the Agent tool
(subagent_type: "sales-os:pipeline-manager"). Pass the user's request plus business context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/pipeline-manager.md`
and follow its workflow inline instead.
