# PO-OS — User Guide

The **Product Owner Operating System** is a multi-agent Claude Code plugin that turns regulatory changes, jurisdictional requirements, and customer signals into a prioritized, DoR-compliant product backlog.

Built for compliance SaaS teams — throughout this guide we use the fictional example **Aurora**, a compliance-tracking SaaS by Acme Studio (`acme-studio/aurora`) — where "what should we build next?" is answered by triangulating three hard-to-reach lenses: what regulators require, what local markets demand, and what customers actually want.

## Architecture

The orchestrator is a skill that runs in your main conversation; the three specialists are **agents** (in `agents/`) invoked via the Agent tool with `subagent_type: "po-os:<agent>"`. Each specialist also keeps a thin trigger skill so direct invocation (`/po-os:regulatory-po` etc.) still works.

```
┌──────────────────────────────────────────┐
│               Lead PO                    │   ← entry point; routes + synthesizes
│         (orchestrator skill)             │
└─────────────┬────────────────────────────┘
              │ Agent tool (subagent_type: "po-os:<agent>")
      ┌───────┼────────────────┐
      ▼       ▼                ▼
 regulatory  localization   discovery-voc
    -po         -po           (agents/)
      │
      │  cross-OS delegation (when enabled, with graceful fallback)
      ▼
┌────────────────────┐     ┌────────────────────────┐
│  legal-os agents   │     │  marketing-os agents   │
│  regulatory-intel  │     │  analytics-reporter    │
│  dpo               │     │  competitive-intel     │
└────────────────────┘     └────────────────────────┘
```

**FAST MODE:** when a request maps to exactly one specialist, the Lead PO delegates to that one agent and relays its output — no synthesis pass. Multi-agent pipelines only run for genuinely multi-domain requests (and the Lead PO says which agents will run first).

Every specialist returns a **Backlog Item** in a shared format. The Lead PO scores, deduplicates, and (with your approval) writes to GitHub Issues.

## Roster

| Component | Kind | Purpose | Slash command |
|---|---|---|---|
| **Lead PO** | Skill (orchestrator) | Routes, scores, synthesizes | `/po-os:lead-po` |
| **Regulatory PO** | Agent | GDPR / CSRD-ESRS / AI Act / NIS2 / DORA / ePrivacy → backlog | `/po-os:regulatory-po` |
| **Localization PO** | Agent | National DPA guidance + jurisdictional overlays → backlog | `/po-os:localization-po` |
| **Discovery & VoC PO** | Agent | Customer signals (support, churn, reviews, forums, interviews) → backlog | `/po-os:discovery-voc` |
| **Setup** | Skill | Configuration wizard | `/po-os:setup` |

## Models & cost

Specialist agents are tiered by the cost of being wrong:

| Agent | Model | Effort | Why |
|---|---|---|---|
| `regulatory-po` | `inherit` | `high` | Regulatory interpretation is judgment-heavy and error-costly — a wrong reading baked into acceptance criteria becomes customer compliance risk. Inherits your session model. |
| `localization-po` | `sonnet` | `medium` | Playbook-driven jurisdictional mapping against curated DPA profiles and checklists. |
| `discovery-voc` | `sonnet` | `medium` | Playbook-driven VoC synthesis: clustering, strength scoring, pattern reports. |

**Portability note:** Model aliases resolve on official Anthropic backends. If you run Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via `ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend" in the [marketplace README](../../README.md#using-the-plugins-on-a-proxied-backend).

## Getting Started

### 1. Install the plugin

Already installed if `po-os` appears in `~/.claude/settings.json` under `enabledPlugins`.

### 2. Configure your product

```
/po-os:setup
```

Walk through 12 steps covering product basics, personas, markets, frameworks, competitors, backlog home, DoR, VoC sources, prioritization weights, cross-OS integration, and optional connected tools. Output: `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`; see "Data Directory" below).

**No config yet?** PO-OS won't hard-stop. Any agent will offer a 3-question inline quick-setup (product, markets, frameworks) or proceed with whatever context you gave, noting reduced personalization.

### 3. Run the Lead PO

```
/po-os:lead-po  "Quarterly backlog refresh for Q3"
```

Or ask directly — the Lead PO classifies and routes:

- "What does the AI Act mean for our product?"
- "What's the Italian market missing?"
- "Why are people churning?"

### 4. Approve backlog item creation

The Lead PO drafts Backlog Items and **always asks before writing** to GitHub Issues. Review the draft, tweak, confirm.

## Backlog Item Format

Every specialist produces the same format (full templates in `skills/lead-po/references/output-formats.md`): title, user story, problem statement, source signal (with citations), acceptance criteria (Given/When/Then), risks & assumptions, priority signals table, dependencies, suggested labels.

## Prioritization Formula

Configurable via `prioritization_weights` in `config.json` (integers 1-5):

```
score =
    regulatory_urgency * urgency_signal (0-1)
  + customer_pain      * pain_signal    (0-1)
  + strategic_fit      * fit_signal     (0-1)
  + effort_inverse     * (1 - effort_normalized)
  + revenue_impact     * revenue_signal (0-1)

P0: score ≥ 0.80 × max_score   P1: ≥ 0.60   P2: ≥ 0.40   P3: < 0.40
```

## Definition of Ready (DoR)

Every backlog item must pass the DoR before receiving the `ready` label:

1. User story in "As a … I want … so that …" form
2. At least 2 acceptance criteria (Given/When/Then preferred)
3. Source signal cited (regulation article, ticket ID, review URL, interview note)
4. Risks and assumptions listed
5. T-shirt size estimate (XS/S/M/L/XL)
6. No unresolved open questions (else: `needs-clarification` label)

## Integrations & Fallbacks

PO-OS never fails hard on a missing integration (details in `references/startup-protocol.md`):

| Integration | Use | Fallback |
|---|---|---|
| `gh` CLI | GitHub Issues backlog | Local `backlog/index.json` + draft shown |
| Web-scraping MCP server | Review/forum extraction | `WebSearch` + `WebFetch` |
| Cloud drive / file MCP | Remote VoC logs | Paste/export from the user |
| claude-mem MCP | Cross-session memory (`po:` prefix) | Proceed without memory |
| `legal-os` / `marketing-os` | Shared regulatory & market intel | Native web research |

MCP tool names are discovered at runtime — nothing is hardcoded. PO-OS's optional integrations follow the same discover-at-runtime rule: if a relevant connector is available as an MCP server in your session, agents may offer to read from it — nothing is configured, no credentials are stored, and you can disable probing with `connectors.enabled: false` in your config.

## Cross-OS Integration

PO-OS **reuses** knowledge held by sibling plugins rather than duplicating it. Cross-OS calls go through the Agent tool with the sibling's scoped agent name; if the sibling is missing, the specialist falls back to native research.

If `legal-os` is installed:
- `regulatory-po` can call `legal-os:regulatory-intel` (`Agent(subagent_type: "legal-os:regulatory-intel", ...)`) for horizon-scan data, then re-interpret it through a product lens
- Heavy GDPR-procedural questions can be punted to `legal-os:dpo`

If `marketing-os` is installed:
- Competitive positioning from `marketing-os:competitive-intel`
- Funnel drop-off metrics from `marketing-os:analytics-reporter`

Configure in setup step 8 — specialists check `cross_os.legal_os` / `cross_os.marketing_os` before delegating. Memory namespaces stay separate: `los:` (legal-os), `mos:` (marketing-os), `po:` (po-os).

## Typical Workflows

### A. Regulatory change lands

1. "AI Act Art. 50 transparency rules take effect in 6 months — what do we need?"
2. Lead PO → `po-os:regulatory-po` (FAST MODE — single agent)
3. The agent lazily reads `references/ai-act-product-mapping.md` (AI question → AI Act mapping only), optionally calls `legal-os:regulatory-intel`
4. Returns 2-3 Backlog Items (e.g., "End-user AI disclosure component", "Synthetic-content watermark API")
5. Lead PO scores; high urgency due to deadline; you approve → GitHub Issues created

### B. Market expansion assessment

1. "Can we launch in Germany next quarter?"
2. Lead PO → `po-os:localization-po`, which runs `references/localization-checklist.md` and reads only the Germany section of `references/national-dpas.md`
3. Returns a **Localization Brief** with GO/CONDITIONAL/NO-GO + derived items

### C. Customer signal synthesis

1. "What are users saying this month?"
2. Lead PO → `po-os:discovery-voc`, which reads enabled sources (support log, competitor G2 reviews, forums)
3. Clusters into named patterns with strength scores; strong signals become backlog items, weak hypotheses go to memory

### D. Full quarterly refresh

1. "Run a Q3 backlog refresh"
2. Lead PO announces, then runs all three agents in parallel (Collaborative model)
3. Synthesizes a **Quarterly Product Plan** with top-10 prioritized items

## Data Directory

- `<DATA_DIR>/config.json` — product profile (schema: `skills/setup/references/config-schema.md`)
- `<DATA_DIR>/backlog/index.json` — local backlog cache
- `<DATA_DIR>/discovery-log.json` — VoC observation append-log

### Where your data lives

`<DATA_DIR>` resolves in order: (1) `$OS_HUB_DATA_DIR/po-os/` if the `OS_HUB_DATA_DIR` environment variable is set, (2) `./os-data/po-os/` in your current working folder if it exists, (3) `~/.claude/plugins/data/po-os/` — the Claude Code default. In Cowork, connect a business folder and your data lands in `./os-data/po-os/` inside it. If your working folder is a git repository, add `os-data/` to its `.gitignore` — it will contain business data (backlog items, discovery logs, customer signals). All data stays local.

## Reference Library (lazy-loaded)

Shared references in `references/` are read by agents **only when the task needs them** — e.g., the AI Act mapping only for AI questions, a national DPA profile only for the requested country:

`startup-protocol.md` · `gdpr-product-mapping.md` · `csrd-esrs-product-mapping.md` · `ai-act-product-mapping.md` · `national-dpas.md` · `localization-checklist.md` · `voc-sources.md` · `synthesis-template.md`

## Known Limitations

- **Regulatory interpretations are AI-assisted, not legal advice** — see [SAFETY.md](SAFETY.md).
- **VoC signal quality is stage-dependent.** Early-stage products with thin data get hypotheses, not facts; sample sizes and biases are always disclosed.
- **GitHub Issues is the default backlog home.** Linear / Jira support is a future addition.
- **No auto-scheduled runs yet.** Use a scheduling skill or cron to automate quarterly refreshes.

## Safety

All PO-OS outputs are AI-assisted product recommendations. Human PO review is required before committing engineering effort, and **regulatory-driven features need counsel review before commitment**. Full statement: [SAFETY.md](SAFETY.md).
