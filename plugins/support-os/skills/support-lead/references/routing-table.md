# Support Lead — Full Routing Table

Delegation is always via the Agent tool: `Agent(subagent_type: "support-os:<agent>", prompt: "<task + config context>")`.

## Single-agent routes (FAST MODE)

| Example request | Agent | Context to pass |
|---|---|---|
| "Triage these tickets" / "Here's my Zendesk export" | `ticket-triage` | PRODUCT, SLA, ESCALATION, the pasted text or CSV path |
| "What's urgent in my inbox?" | `ticket-triage` | Same as above |
| "Reply to this customer" / "Answer this email" | `response-drafter` | PRODUCT, VOICE, SLA, REFUND_POLICY, CROSS_OS.marketing_os, ticket text |
| "This customer is furious — help" | `response-drafter` | Same + note it's a hard case |
| "They want a refund" | `response-drafter` | Same; drafter prepares approve+decline versions when above policy |
| "Write the outage apology" | `response-drafter` | Same + incident facts from the user |
| "Save this as a canned response" / "Show my saved replies" | `response-drafter` | PRODUCT, VOICE |
| "Turn this ticket into a help article" | `kb-writer` | PRODUCT, VOICE, KB, resolved thread text |
| "Update the FAQ" / "Audit the knowledge base" | `kb-writer` | PRODUCT, KB |
| "What should we document next?" | `kb-writer` | PRODUCT, KB, recent triage output or ticket batch |
| "What are the support trends?" / "Top complaints this month" | `insights-analyst` | PRODUCT, CROSS_OS.po_os, ticket batch or CSV |
| "Any churn signals?" | `insights-analyst` | Same |
| "Which parts of the product generate the most bugs?" | `insights-analyst` | Same |

## Multi-agent routes (announce agents before running)

| Request | Pipeline | Notes |
|---|---|---|
| "Handle my inbox" / "Work through these tickets" | `ticket-triage` → `response-drafter` | Only `answer-now` items get drafts; `user-decision` items go straight to the user; `kb-link` gaps are listed for later |
| "Monthly support review" | `insights-analyst` → `kb-writer` | Trends first; kb-writer turns recurring questions into a writing queue |
| "Full support health check" | `ticket-triage` + `insights-analyst` in parallel, then `kb-writer` + `response-drafter` on their outputs | Collaborative; synthesize per output-formats.md |
| "Prep this trend for the product backlog" | `insights-analyst`, then hand off to po-os `lead-po`/`discovery-voc` if installed | Cross-OS; fall back to direct presentation |

## Anti-routes (do NOT delegate)

- Config/onboarding questions → `/support-os:setup`.
- Pure product-backlog prioritization → po-os (if installed); support-os only supplies the signal.
- Legal advice on a threatening customer email → flag as `user-decision`, suggest legal-os or counsel; never draft legal positions.
