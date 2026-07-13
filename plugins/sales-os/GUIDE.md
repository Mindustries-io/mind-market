# Sales OS — User Guide

A sales operating system for a one-person business. One orchestrator (the Sales Director), four specialist agents, a pipeline file you own — all from your CLI.

**Where it sits in the family:** sales-os is **1-to-1** — named prospects, individual deals, personal outreach, proposals. Its sibling marketing-os is **1-to-many** — audience, content, campaigns. "Cold email to Jordan at Acme Studio" is sales-os; "welcome email sequence for signups" is marketing-os.

---

## Quick Start

### 1. Run setup

```
/sales-os:setup
```

The wizard captures: your offer and services · pricing/rates · ideal customer profile (ICP) · pipeline stages and file location · outreach tone and do-not-contact list · proposal defaults (terms, tiers, validity) · your CRM situation · which sibling OS plugins you have.

Config is saved to `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`). Multiple business profiles are supported. If you skip setup, agents still work — they'll offer a 3-question quick-setup or proceed with reduced personalization.

### Where your data lives

Your config and data files live in `<DATA_DIR>`, resolved in this order: `$OS_HUB_DATA_DIR/sales-os/` if that environment variable is set → `./os-data/sales-os/` when `./os-data/` exists in your working folder (the per-OS subfolder is created as needed) → `~/.claude/plugins/data/sales-os/` (the Claude Code default). In Cowork, connect a business folder and create an `os-data/` folder inside it (setup and migrate offer to do this) — your data then lands in `./os-data/sales-os/`. If your working folder is a git repository, add `os-data/` to its `.gitignore` — it will contain business data (deals, prospects, win/loss history).

### 2. Talk to the Sales Director

```
/sales-os:sales-director [your request]
```

Examples:

```
/sales-os:sales-director Weekly pipeline review
/sales-os:sales-director Cold email to the Head of Ops at Acme Studio
/sales-os:sales-director Draft a proposal for the Aurora dashboard project
/sales-os:sales-director Which deals are going stale?
/sales-os:sales-director Turn these call notes into a CRM update: ...
/sales-os:sales-director We lost the Northwind deal to price — log it
```

### 3. Go direct to a specialist

```
/sales-os:pipeline-manager Move the Aurora deal to negotiation
/sales-os:outreach-writer 4-touch follow-up sequence for the stalled deal
/sales-os:proposal-drafter Quote for a two-week audit engagement
/sales-os:crm-hygienist Dedupe this contacts export: [paste]
```

---

## The Team

| Agent | Domain | When to use |
|---|---|---|
| **Sales Director** (orchestrator) | Routing & synthesis | You're not sure who to ask, or the task spans specialists |
| **Pipeline Manager** | Deal tracking | Stages, next actions, stale flags, weekly review, win/loss log |
| **Outreach Writer** | 1-to-1 outreach | Cold/warm emails and DMs to named people, follow-up sequences, breakups |
| **Proposal Drafter** | Closing paperwork | Proposals, quotes, SOWs, pricing tiers, scope change requests |
| **CRM Hygienist** | Data quality | Dedupe/normalize exports, meeting notes → structured updates, next-action audits |

The Sales Director runs in **fast mode** by default: single-specialist requests go to one agent and come straight back. Multi-agent pipelines (e.g. "review my pipeline and draft chasers for the stale deals") are announced before they run.

---

## Models & cost

Agents are tiered so routine work stays cheap:

| Agent | Model | Effort | Why |
|---|---|---|---|
| pipeline-manager | `haiku` | low | Mechanical: file updates, date math, status tables |
| crm-hygienist | `haiku` | low | Mechanical: parsing, dedupe, normalization |
| outreach-writer | `sonnet` | medium | Playbook-driven drafting with research and voice matching |
| proposal-drafter | `sonnet` | medium | Playbook-driven drafting with pricing structure judgment |

**Portability note:** Model aliases resolve on official Anthropic backends. If you run Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via `ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend" in the [marketplace README](../../README.md#using-the-plugins-on-a-proxied-backend).

---

## Architecture

```
/sales-os:sales-director            (orchestrator skill — routing, fast mode, synthesis)
   ├── Agent → sales-os:pipeline-manager    (agents/pipeline-manager.md)
   ├── Agent → sales-os:outreach-writer     (agents/outreach-writer.md)
   ├── Agent → sales-os:proposal-drafter    (agents/proposal-drafter.md)
   └── Agent → sales-os:crm-hygienist       (agents/crm-hygienist.md)
/sales-os:<specialist>              (thin trigger skills → same agents)
/sales-os:setup                     (config wizard)
references/                         (startup protocol + lazy-loaded playbooks/templates)
```

Your pipeline lives in a plain markdown file you own (default `<DATA_DIR>/pipeline.md`, configurable) — no SaaS lock-in. CRMs, if you have one, are treated as read-only sources via exports.

---

## Integrations (all optional)

| Source | Provides | Fallback if missing |
|---|---|---|
| CRM MCP server (HubSpot / Pipedrive / Attio) | Deal & contact reads | Paste a CSV export — credentials are never requested |
| WebSearch | Prospect research for personalization | You describe the prospect; drafts are shorter and honest about it |
| claude-mem (memory) | Cross-session deal context, win/loss history | Pipeline file alone carries state |
| marketing-os | Brand voice reuse in outreach | sales-os `outreach.tone` config |
| legal-os | Contract-manager escalation for custom terms | Recommendation to get human lawyer review |
| po-os | Win/loss reasons feed discovery-voc | Reasons stay in `sal:` memory for later |

**Live data when connected, exports otherwise:** if a relevant connector (bank, payments, CRM, helpdesk, SEO...) is available as an MCP server in your session, agents will offer to read from it directly; otherwise everything works from pasted exports. Discovery is at runtime — nothing is configured, no credentials are stored, and you can disable it with `connectors.enabled: false` in your config.

---

## Common workflows

**Monday review:**
```
/sales-os:sales-director Weekly pipeline review, then draft follow-ups for anything stale
```

**New lead to close:**
```
1. /sales-os:outreach-writer Cold email to [person] at [company]
2. /sales-os:sales-director They replied — draft a response and add the deal to my pipeline
3. /sales-os:proposal-drafter Proposal with 3 options for [project]
4. /sales-os:sales-director They accepted option B — SOW and move to won
```

**Quarterly hygiene:**
```
1. /sales-os:crm-hygienist Clean this export: [paste]
2. /sales-os:pipeline-manager Import the cleaned deals and flag missing next actions
3. /sales-os:sales-director Win/loss analysis for the quarter
```

---

## Outreach compliance note

Sales OS drafts outreach; **you send it and own compliance**. E-marketing and anti-spam rules vary by country (GDPR/ePrivacy in the EU — some member states require opt-in even for B2B; CAN-SPAM in the US; CASL in Canada). The outreach-writer flags strict jurisdictions, never uses deceptive subjects or fake personalization, and honors your do-not-contact list absolutely — but a flag is not legal advice. When in doubt about a market's rules, check locally (or ask legal-os if installed). See `SAFETY.md` for the consolidated disclaimer.

---

## Troubleshooting

- **"Config not found"** — run `/sales-os:setup`, or answer the inline quick-setup when an agent offers it.
- **Wrong business context** — check `active_profile` in `<DATA_DIR>/config.json`.
- **Agent asks for a CRM export** — that's by design; Sales OS never takes CRM credentials.
- **Pipeline file missing** — any pipeline task offers to create it from the built-in template.
