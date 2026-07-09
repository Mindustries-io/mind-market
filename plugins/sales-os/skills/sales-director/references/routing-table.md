# Sales Director — Full Routing Table

Delegation is always `Agent(subagent_type: "sales-os:<agent>", prompt: ...)`. FAST MODE: one matching row → one agent → relay.

## pipeline-manager (haiku)

| Example prompt | Notes |
|---|---|
| "What's in my pipeline?" / "pipeline review" | Weekly-review workflow |
| "Move Aurora deal to negotiation" | Stage move, updates pipeline file |
| "Which deals are going stale?" | Stale sweep |
| "Log the Aurora deal as won" / "we lost the Northwind deal to price" | Win/loss log + memory + po-os feed |
| "How much pipeline value do I have this quarter?" | Snapshot from pipeline file |
| "Set up a pipeline for me" | Creates file from pipeline-template.md |
| "Pull my deals from HubSpot" | CRM export path — never credentials |

## outreach-writer (sonnet)

| Example prompt | Notes |
|---|---|
| "Cold email to Jordan Reyes, Head of Ops at Acme Studio" | Research-first cold opener |
| "Follow up with the prospect who went quiet" | Nudge; pairs with pipeline-manager for context |
| "Write a 4-touch follow-up sequence for the Aurora deal" | Sequence workflow |
| "Breakup email for the stalled Northwind deal" | Final-touch draft |
| "They replied asking about pricing — draft a response" | Reply handling; may hand off to proposal-drafter |
| "LinkedIn DM to a founder I met at the conference" | 1-to-1 DM |

**NOT this agent:** newsletters, drip campaigns, list blasts → marketing-os `email-campaigns` (1-to-many).

## proposal-drafter (sonnet)

| Example prompt | Notes |
|---|---|
| "Draft a proposal for Acme Studio's compliance-dashboard project" | 3-tier default |
| "Quote for a two-week audit engagement" | Quote with validity date |
| "Turn the accepted proposal into a SOW" | SOW workflow |
| "Client wants two more features mid-project" | Change-request draft |
| "Give me pricing options for a retainer" | Tier structuring |

**Escalation:** bespoke legal clauses → `legal-os:contract-manager` if installed, else human lawyer.

## crm-hygienist (haiku)

| Example prompt | Notes |
|---|---|
| "Here's my Pipedrive export, clean it up" | Dedupe & normalize |
| "I pasted my contacts twice, find duplicates" | Dedupe clusters, confirm merges |
| "Turn these call notes into a CRM update" | Notes → structured block |
| "Which deals have no next action?" | Next-action audit |

## Multi-agent patterns

| Request shape | Flow | Model |
|---|---|---|
| Review + chase | pipeline-manager → outreach-writer | Pipeline |
| Notes → pipeline update | crm-hygienist → pipeline-manager | Pipeline |
| Call prep briefing | pipeline-manager + outreach-writer (parallel) → synthesize | Collaborative |
| Won deal → paperwork | pipeline-manager + proposal-drafter (parallel) | Collaborative |
| Import + audit | crm-hygienist → pipeline-manager | Pipeline |

## Redirects (not sales-os)

| Request | Send to |
|---|---|
| Newsletter, campaign, audience content, social posts | marketing-os (`/marketing-os:cmo`) |
| Contract redlining, NDA, DPA | legal-os `contract-manager` |
| "What should we build?" from win/loss patterns | po-os `discovery-voc` / `lead-po` |
