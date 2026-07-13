# Legal OS — User Guide

A legal operating system for Claude Code: one orchestrator skill (the General Counsel)
and seven specialist **agents**, purpose-built for EU compliance-focused SMEs.

> All output is AI-assisted work product, not legal advice — see [SAFETY.md](SAFETY.md).

---

## Quick start

```
/legal-os:setup                       # configure your organization (once)
/legal-os:general-counsel <request>   # single entry point — routes for you
```

Try:

```
/legal-os:general-counsel What's our current compliance status?
/legal-os:general-counsel Do we need a DPIA for our new analytics system?
/legal-os:general-counsel Review this NDA
/legal-os:general-counsel We had a data breach — a laptop was stolen
/legal-os:general-counsel Run a full legal health check
```

Setup is optional to get started: if no config exists, agents offer a 3-question inline
quick-setup or proceed from the context you give them. The full wizard
(`/legal-os:setup`) saves your profile to `<DATA_DIR>/config.json` (see "Where your
data lives" under Setup).

You can also go direct — every specialist has a trigger skill that hands off to its
agent: `/legal-os:dpo`, `/legal-os:contract-manager`, `/legal-os:compliance-officer`,
`/legal-os:legal-researcher`, `/legal-os:regulatory-intel`, `/legal-os:vendor-risk`,
and `/legal-os:incident-response` (invokable directly in an emergency — no routing delay).

---

## Architecture

```
You ──> general-counsel (skill, main conversation)
              │  classifies the request, then delegates via the Agent tool
              ▼
        legal-os:<agent>  (subagents in agents/*.md)
        ├── compliance-officer    status, obligations, scores, evidence gaps
        ├── dpo                   GDPR analysis: DPIA, DSAR, lawful basis, transfers
        ├── contract-manager      review, redlines, NDA triage, DPA (Art. 28), portfolio
        ├── legal-researcher      research, IRAC memos, case law, comparisons
        ├── regulatory-intel      horizon scans, enforcement, deadlines, impact
        ├── incident-response     breach triage, 72h workflow, notifications, drills
        └── vendor-risk           8-domain vendor scoring, sub-processors, audits
```

**FAST MODE (default):** a single-domain request goes to exactly one agent and its
answer is relayed as-is — no synthesis pass, no extra agents. Three delegation models
exist, with **hub-and-spoke as the default**:

| Model | When | Example |
|---|---|---|
| A. Hub-and-spoke | Single-domain request (default) | "What's our compliance score?" → compliance-officer |
| B. Pipeline | One agent's output feeds the next | Breach: incident-response → dpo; Vendor onboarding: vendor-risk → contract-manager |
| C. Collaborative | Genuinely multi-domain | Board report: compliance-officer + dpo + contract-manager + regulatory-intel, then synthesis |

For pipelines and fan-outs the GC announces which agents will run before running them.
The comprehensive **legal health check** runs all seven agents and produces a graded
report (A-F, risk heat map, top-5 priorities).

All agents share one startup protocol (`references/startup-protocol.md`): config load
with graceful degradation, optional memory (prefix `los:`), standard variables, and
tool fallbacks. Deep-dive references in `references/` are loaded lazily — e.g. the memo
templates only when drafting a memo, notification templates only during an actual breach.

---

## Models & cost

Agents are tiered by the kind of work they do:

| Agent | Model | Effort | Why |
|---|---|---|---|
| compliance-officer | `haiku` | low | Mechanical status lookups, filtering, and report formatting; escalates judgment calls to the dpo agent |
| legal-researcher | `sonnet` | medium | Playbook-driven research and drafting against a fixed source hierarchy |
| vendor-risk | `sonnet` | medium | Structured scoring against a defined 8-domain matrix |
| dpo | `inherit` | high | Judgment-heavy, error-costly regulatory interpretation (DPIAs, Art. 33/34 decisions) |
| contract-manager | `inherit` | high | Redlining and risk calls with real financial consequences |
| incident-response | `inherit` | high | Time-critical breach decisions under the 72-hour clock |
| regulatory-intel | `inherit` | high | Interpreting ambiguous new regulation for org-specific impact |

`inherit` keeps your session model, so those agents also work on proxied or third-party
backends.

**Portability note:** Model aliases resolve on official Anthropic backends. If you run
Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via
`ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the
agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is
ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend"
in the [marketplace README](../../README.md#using-the-plugins-on-a-proxied-backend).

---

## Setup

`/legal-os:setup` walks through: organization basics, jurisdictions and frameworks
(GDPR, NIS2, AI Act, DORA, CSRD, ePrivacy...), DPO and supervisory authority, optional
compliance tracker, contract defaults (your negotiation playbook), risk appetite, and
reporting preferences. Multiple organization profiles are supported
(`/legal-os:setup switch to <slug>`).

### Where your data lives

`<DATA_DIR>` resolves in order: `$OS_HUB_DATA_DIR/legal-os/` (if the env var is set) →
`./os-data/legal-os/` in your working folder (if it exists) →
`~/.claude/plugins/data/legal-os/` (the Claude Code default). In Cowork, connect a
business folder so your data lands in `./os-data/legal-os/` inside it. If your working
folder is a git repository, add `os-data/` to its `.gitignore` — it will contain
business data (compliance snapshots, contract records, incident logs).

All your data stays local under `<DATA_DIR>`:

| File | Purpose |
|---|---|
| `config.json` | Organization legal profile |
| `compliance-snapshot.json` | Compliance tracker export (optional) |
| `contracts/index.json` | Contract portfolio |
| `regulatory-calendar.json` | Compliance deadlines |
| `incident-log.json` | Incident history |
| `vendor-registry.json` | Vendor tracking |

---

## Integrations (all optional, all with fallbacks)

| Integration | What it adds | Fallback if missing |
|---|---|---|
| **Compliance tracker export** — a JSON/CSV snapshot you export from whatever tool you use (e.g. the fictional Aurora by Acme Studio) and save to `compliance-snapshot.json` | Precise obligation status, scores, evidence gaps | **Lite mode:** paste your status data; agents give qualitative assessments and clearly labeled estimates |
| **Memory MCP server** (e.g. claude-mem) | Cross-session legal context under the `los:` prefix | Agents proceed without memory |
| **Web-scraping MCP server** (e.g. an Apify-style RAG web browser) | Deeper extraction from EUR-Lex, EDPB, DPA portals | `WebSearch` + `WebFetch` — fully sufficient |
| **Anthropic `legal:*` skills** (marketplace) | Polished standalone deliverables (contract review, NDA triage, briefings) | Each agent covers the task natively |

Agents discover MCP tool names at runtime and never fail hard on a missing integration.
Legal OS's optional integrations follow the same discover-at-runtime rule as
connector-aware agents across the OS family — nothing is configured, no credentials are
stored, and you can disable probing with `connectors.enabled: false` in your config.
See `references/compliance-data-sources.md` for the snapshot format.

---

## Common workflows

**Compliance status** — `"What's our compliance status?"` → compliance-officer reads
the snapshot (or lite-mode data) and reports score, framework completion, overdue items,
and evidence gaps.

**DPIA** — `"Do we need a DPIA for our employee monitoring system?"` → dpo runs Art. 35
screening; if required, walks the full assessment through mitigation and DPO opinion.

**Contract review** — `"Review this SaaS agreement"` (paste or give a path) →
contract-manager checks every clause against your playbook, rates GREEN/YELLOW/RED,
and produces redlines with alternative language.

**Data breach (emergency)** — `/legal-os:incident-response A laptop with customer data
was stolen` → immediate triage, severity classification, incident log, notification
decision (Art. 33/34), draft notifications, DPO handoff. No routing delay.

**Vendor onboarding (pipeline)** — `"Onboard CloudCorp as a data processor"` →
vendor-risk scores 8 domains → contract-manager reviews the DPA with that context →
compliance tracking flagged → consolidated onboarding report.

**Quarterly board report (collaborative)** — compliance-officer + dpo +
contract-manager + regulatory-intel in parallel, synthesized into a graded Legal
Health Report.

**Tips:** be specific ("overdue GDPR obligations with evidence gaps" beats "check
compliance"); reference articles directly ("Is this DPA Art. 28(3)-complete?"); include
counterparty context when reviewing documents; go direct to a specialist when you know
the domain, use the GC when you don't or when you need multiple perspectives.

---

## Troubleshooting

| Issue | Solution |
|---|---|
| No config found | Answer the inline quick-setup, or run `/legal-os:setup` |
| "Snapshot is stale" | Re-export from your compliance tool to `compliance-snapshot.json`, or use lite mode |
| Wrong org context | `/legal-os:setup show current config`, or switch profiles |
| GC routes to the wrong specialist | Go direct to the specialist skill, or be more specific |
| Skills/agents not loading | Start a new Claude Code session — plugins load at session start |
| Aliases don't resolve on my proxy backend | See the portability note under "Models & cost" |

---

## Safety

Legal OS is an assistant, not counsel. Breach notifications, contract execution, DPIA
approvals, DSAR responses, and anything filed with an authority require qualified human
review first. Full statement: [SAFETY.md](SAFETY.md).
