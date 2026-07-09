---
name: localization-po
description: "Jurisdictional Localization Product Owner specialist. Use when the user asks about country-specific product requirements, national data protection authority expectations (Garante, CNIL, ICO, BfDI, AEPD, DPC, and others), local framework overlays on EU regulations, language/locale requirements, market-entry product readiness, or jurisdictional differences affecting the product backlog. Expert in converting national DPA guidance, enforcement priorities, and local legal overlays into backlog items."
---

Delegate this request to the `po-os:localization-po` subagent via the Agent tool
(subagent_type: "po-os:localization-po"). Pass the user's request plus product context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/localization-po.md`
and follow its workflow inline instead.
