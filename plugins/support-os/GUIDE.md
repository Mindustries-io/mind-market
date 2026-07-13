# Support-OS — User Guide

A customer-support Operating System for solopreneurs: a virtual **Support Lead** plus four specialist agents that triage your inbox, draft replies in your voice, grow a help center from resolved tickets, and mine the whole queue for product signal. Built for a one-person product business where you *are* the support team.

The promise: **no ticket unanswered, no pattern unnoticed, no answer written twice.**

## Quick start

1. Install the plugin from the marketplace (see the repository README).
2. Run `/support-os:setup` — the wizard asks for your product, channels, tone, SLA targets, and refund policy (about 3 minutes). Config is saved locally to `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`) — see "Where your data lives" below.
3. Paste a few tickets (or point at a helpdesk CSV export) and say: *"triage these"* — or paste one email and say *"reply to this customer"*.

Skipped setup? Any skill will offer a 3-question quick-setup inline and keep going.

## Architecture

```
                    ┌─────────────────────────┐
   your request ──▶ │  support-lead           │  orchestrator (routes, FAST MODE,
                    │  (Support Lead)         │  synthesizes multi-agent runs)
                    └───────────┬─────────────┘
              ┌───────────┬─────┴──────┬─────────────┐
              ▼           ▼            ▼             ▼
       ticket-triage  response-    kb-writer   insights-analyst
       (sort, flag,   drafter      (articles,  (trends, churn
        route)        (replies,     FAQs,       signals, bug
                      canned lib)   deflection) hotspots)
```

- **FAST MODE:** if your request maps to one specialist, the Support Lead delegates to exactly that agent and relays the result — no overhead. Multi-agent pipelines run only for genuinely multi-part requests ("handle my inbox", "monthly support review"), and the Lead tells you which agents will run first.
- Each specialist also has its own trigger skill, so "write a canned response for refunds" goes straight to the drafter without a routing hop.

## The agents

| Agent | What it does | Typical prompts |
|---|---|---|
| `ticket-triage` | Severity, topic, sentiment, churn-risk flags, and a routing proposal per ticket. Reads pasted text or helpdesk CSV exports (Zendesk, Help Scout, Freshdesk, Intercom, plain inbox) — never asks for credentials. | "Triage these tickets", "What's urgent?" |
| `response-drafter` | Ready-to-send replies in your brand voice, including hard cases (angry customer, refund, outage apology) with de-escalation patterns. Builds a canned-response library in memory. | "Reply to this customer", "They want a refund", "Write the outage apology" |
| `kb-writer` | Turns resolved tickets into help-center articles and FAQs, keeps an article inventory, and ranks what to write next by ticket-deflection potential. | "Turn this into a help article", "What should we document next?" |
| `insights-analyst` | Mines ticket batches for trends: pain points, feature-request clusters, churn signals, bug hotspots — with honest sample sizes. Feeds po-os if installed. | "Support trends this month?", "Any churn signals?" |

**Decision boundaries:** the agents draft; you send and publish. Refunds above your policy threshold, legal threats, and security reports are always routed to you with a recommendation — never decided by an agent.

## Models & cost

| Agent | Model | Effort | Why |
|---|---|---|---|
| `ticket-triage` | `haiku` | low | Mechanical categorization against a fixed rubric — fast and cheap matters when triaging 50 tickets |
| `response-drafter` | `sonnet` | medium | Playbook-driven drafting where tone and de-escalation quality matter |
| `kb-writer` | `sonnet` | medium | Structured writing and inventory synthesis |
| `insights-analyst` | `sonnet` | medium | Cross-ticket clustering and synthesis |
| `support-lead` (orchestrator) | your session model | — | Runs in the main conversation |

**Portability note:** Model aliases resolve on official Anthropic backends. If you run Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via `ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend" in the [marketplace README](../../README.md#using-the-plugins-on-a-proxied-backend).

Cost shape: a typical "handle my inbox" session is one cheap haiku pass over the whole batch plus sonnet drafts only for the tickets that need real replies.

## Integrations (all optional, all with fallbacks)

| Integration | Used for | Fallback if missing |
|---|---|---|
| Helpdesk tool (Zendesk, Help Scout, Freshdesk, Intercom, …) | Interpreting **local CSV exports** only — support-os never connects to helpdesk APIs or asks for credentials | Paste ticket text directly |
| marketing-os plugin | Reusing your configured brand voice for replies and articles | `voice` section of support-os config; else a friendly-professional default |
| po-os plugin | `insights-analyst` emits findings as Discovery Insights that po-os `discovery-voc` consumes for backlog synthesis | Same findings presented directly to you, labeled as product signal |
| Memory tool (e.g. claude-mem MCP) | Canned-response library, KB inventory, trend history under the `sup:` prefix | Everything still works per-session; durable libraries are re-seeded from your files |
| WebSearch / WebFetch | Occasional public-docs lookups | Paste the relevant text |

**Live data when connected, exports otherwise:** if a relevant connector (bank, payments, CRM, helpdesk, SEO...) is available as an MCP server in your session, agents will offer to read from it directly; otherwise everything works from pasted exports. Discovery is at runtime — nothing is configured, no credentials are stored, and you can disable it with `connectors.enabled: false` in your config.

## Memory

Support-OS stores durable state under the `sup:` prefix: canned responses (`sup: {product} canned: …`), KB inventory (`sup: {product} kb article: …`), trends (`sup: {product} trend: …`), promised follow-ups, and Discovery Insights for po-os. Sibling OSs use their own prefixes (`po:`, `mos:`, …) — support-os only writes to `sup:`.

## Configuration

Full schema: `skills/setup/references/config-schema.md`. Highlights:

- `voice` — tone, signoff, words to avoid, and whether marketing-os voice wins.
- `sla` — first-response and resolution targets; agents flag at-risk tickets.
- `refund_policy` — summary, window, and the threshold above which *you* decide. The drafter prepares approve *and* decline versions for anything above it.
- `escalation.user_decides` — the list of things agents must always hand to you.
- `cross_os` — `po_os` / `marketing_os` flags.

Multiple products are supported via profiles; switch with `/support-os:setup`.

### Where your data lives

Config and data files resolve in this order: (1) `$OS_HUB_DATA_DIR/support-os/` if that environment variable is set, (2) `./os-data/support-os/` in your current working folder if it exists, (3) `~/.claude/plugins/data/support-os/` (the Claude Code default). In Cowork, connect a business folder so your data lands in `./os-data/support-os/` inside it. If your working folder is a git repository, add `os-data/` to its `.gitignore` — it will contain business data (customer tickets, complaint threads, canned responses).

## Example session

```
You:  Here's my Help Scout export from June. What happened, and what should I do first?
Lead: Running ticket-triage and insights-analyst in parallel, then synthesizing.
      → 41 tickets: 1 needs your decision (refund on an annual plan — recommend
        pro-rated credit), 6 ready-to-send drafts, 3 recurring questions worth a
        KB article ("confused by invoice line items", asked 7×, strong signal),
        and 1 churn watchlist entry. Full report below…
```

## Privacy

Ticket data may contain personal data — keep helpdesk exports local, paste only what's needed, and support-os strips names/emails from anything it stores in memory.

All outputs are AI-assisted drafts and analyses; review before sending, publishing, or making commitments to customers.
