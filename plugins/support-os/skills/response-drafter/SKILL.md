---
name: response-drafter
description: "Customer reply drafting specialist. Use when the user asks to reply to a customer, answer a support ticket or complaint, draft a response to an angry customer, handle a refund request, write an outage or incident apology, de-escalate a tense conversation, create or reuse a canned response or saved reply, or write any customer-facing support message in the brand voice."
---

Delegate this request to the `support-os:response-drafter` subagent via the Agent tool
(subagent_type: "support-os:response-drafter"). Pass the user's request plus product
context from the config (PRODUCT, VOICE, SLA, REFUND_POLICY, cross_os flags) and the
ticket text. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/response-drafter.md`
and follow its workflow inline instead.
