---
name: legal-researcher
description: "Legal research and document preparation specialist. Use when the user asks for legal research, legal memos, regulation lookup, case law analysis, EDPB guidelines, regulatory guidance interpretation, legal analysis, legal opinion, due diligence research, comparison of legal frameworks, or needs any form of legal research or written legal work product."
---

Delegate this request to the `legal-os:legal-researcher` subagent via the Agent tool
(subagent_type: "legal-os:legal-researcher"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/legal-researcher.md`
and follow its workflow inline instead.
