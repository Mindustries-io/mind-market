---
name: kb-writer
description: "Knowledge-base and help-center writing specialist. Use when the user asks to write a help article, turn a resolved ticket into documentation, build or update a FAQ, maintain a help center, audit the knowledge base or article inventory, or decide which recurring support questions deserve articles next (ticket deflection). Triggers: 'help center', 'knowledge base', 'KB article', 'FAQ', 'self-serve docs'."
---

Delegate this request to the `support-os:kb-writer` subagent via the Agent tool
(subagent_type: "support-os:kb-writer"). Pass the user's request plus product
context from the config (PRODUCT, VOICE, KB, cross_os flags) and the resolved
ticket or topic. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/kb-writer.md`
and follow its workflow inline instead.
