# Security OS — Virtual CISO for Solopreneurs

Security OS is a defensive-security Operating System for a one-person software/SaaS business. It gives you a virtual CISO that routes security work to five specialist agents: application security review, threat modeling, posture hardening, vendor due diligence, and incident response.

**Defensive only.** Security OS reviews and protects what you own. It does not build exploits, attack tooling, or test systems you don't own. See `SAFETY.md`.

## Quick Start

1. Install the plugin from the marketplace (see the repository README).
2. Run the setup wizard: ask for **"security os setup"** (or `/security-os:setup`). It captures your org profile, stack inventory, crown-jewel data, and existing controls in about five minutes.
3. Get a baseline: **"run a security posture check"** — the hardening auditor scores your setup and gives you a top-5 action list.
4. From then on, just talk to the CISO: "review the security of my API", "threat model this feature", "is this vendor safe?", "I think my key leaked".

Configuration lives at `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`) — local to your machine, never uploaded anywhere by the plugin.

### Where your data lives

Security OS resolves its data directory in this order: `$OS_HUB_DATA_DIR/security-os/` if the `OS_HUB_DATA_DIR` environment variable is set; else `./os-data/security-os/` when `./os-data/` exists in your working folder (the per-OS subfolder is created as needed); else `~/.claude/plugins/data/security-os/` (the Claude Code default). In Cowork, connect a business folder and create an `os-data/` folder inside it (setup and migrate offer to do this) — your data then lands in `./os-data/security-os/`. If your working folder is a git repository, add `os-data/` to its `.gitignore` — it will contain business data (org profile, stack inventory, crown-jewel list).

## Architecture

```
                        ┌────────────────────┐
  "is my app secure?" → │   ciso (skill)     │  routing · FAST MODE · synthesis
                        └─────────┬──────────┘
          ┌───────────┬───────────┼────────────┬──────────────┐
          ▼           ▼           ▼            ▼              ▼
   appsec-      threat-     hardening-    vendor-      security-
   reviewer     modeler     auditor       security     incident
   (code)       (design)    (ops posture) (3rd party)  (emergencies)
```

- The **CISO orchestrator** is the main entry point. In FAST MODE (the default for single-domain requests) it delegates to exactly one specialist and relays the result — no overhead.
- Each specialist also has a **thin trigger skill**, so "threat model my billing feature" goes straight to the right agent without a routing hop.
- The **security-incident** agent is an emergency lane: incident reports route to it immediately, before anything else.
- Shared reference material (checklists, playbooks, templates) is lazy-loaded from `references/` only when a workflow needs it.

## The Specialists

| Agent | What it does | Typical trigger |
|---|---|---|
| `appsec-reviewer` | OWASP Top 10 walkthrough, auth review, secrets scan, dependency vulnerability check on your own code; prioritized findings with file:line and fixes | "security review", "check for vulnerabilities" |
| `threat-modeler` | Lightweight STRIDE threat model of a feature/architecture; threat table with likelihood/impact, mitigations, decisions | "threat model", "what could go wrong" |
| `hardening-auditor` | Scored posture checklist: MFA/passkeys, cloud/SaaS config, backups & restore test, SPF/DKIM/DMARC, endpoint, secrets; top-5 actions | "harden", "posture check" |
| `vendor-security` | Due diligence on third-party tools: SOC 2/ISO 27001 signals, breach history, data-access scope; adopt/avoid verdict | "is this vendor safe" |
| `security-incident` | Containment-first playbook: triage, contain, scope, eradicate, recover, post-mortem | "I've been hacked", "leaked key" |

## Models & cost

| Agent | Model | Effort | Why |
|---|---|---|---|
| `appsec-reviewer` | `inherit` | high | Judgment-heavy, error-costly: a missed auth bypass is expensive |
| `threat-modeler` | `inherit` | high | Design-level risk judgment; wrong ratings misdirect scarce solo time |
| `security-incident` | `inherit` | high | Incident decisions under pressure; mistakes compound |
| `hardening-auditor` | `sonnet` | medium | Playbook-driven checklist work against a fixed reference |
| `vendor-security` | `sonnet` | medium | Structured research and synthesis from public signals |

`inherit` keeps your session model, so the judgment-heavy agents run on whatever you're already paying for — including proxied/third-party backends.

**Portability note:** Model aliases resolve on official Anthropic backends. If you run Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via `ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend" in the [marketplace README](../../README.md#using-the-plugins-on-a-proxied-backend).

## Integrations (all optional, all with fallbacks)

| Integration | Used by | Fallback if missing |
|---|---|---|
| `static-analysis:semgrep` / `static-analysis:codeql` skills | appsec-reviewer | Manual review playbook (`references/appsec-review-checklist.md`) |
| `npm audit`, `pip-audit`, `osv-scanner`, `cargo audit`, `govulncheck` | appsec-reviewer | Paste lockfile; advisory lookup via WebSearch |
| WebSearch / WebFetch | vendor-security, incident research | User pastes vendor trust pages / advisories; assessment flagged lower-confidence |
| Memory (e.g. claude-mem MCP server) | All agents (`sec:` prefix) | Proceeds silently without cross-session memory |
| **legal-os plugin** (cross-OS) | vendor-security, security-incident | Built-in minimal legal reminders instead |

Nothing fails hard on a missing integration — agents state the gap and continue.

Security OS's optional integrations follow the same discover-at-runtime rule as connector-aware plugins: tools are found by name at runtime, nothing is configured, no credentials are stored, and you can disable connector probing with `connectors.enabled: false` in your config.

## Cross-OS: working with legal-os

If the `legal-os` plugin is installed (set `cross_os.legal_os_installed` in setup):

- **Vendors:** `vendor-security` covers the security side and stores findings under `sec:vendor:<slug>` memory keys; `legal-os:vendor-risk` picks them up for the DPA/contract side. One vendor, two lenses, no duplicated work.
- **Incidents:** if personal data may be affected, `security-incident` hands the GDPR 72-hour notification track to `legal-os:incident-response` and keeps driving technical containment.

Without legal-os, both agents fall back to concise built-in reminders of the legal obligations and recommend counsel.

## Example prompts

- "Do a security review of `src/api/` — focus on auth and input handling."
- "Threat model the new file-sharing feature before I build it."
- "Security posture check — score me and tell me the five things to fix first."
- "I want to adopt an AI transcription SaaS that will see customer calls — due diligence, please."
- "I just pushed a live Stripe key to a public repo. Help."
- "Full security health check." (runs all four review agents + synthesis)

## Files

- `GUIDE.md` — this file
- `SAFETY.md` — scope and limitations (read it once)
- `agents/` — the five specialists
- `skills/ciso/` — orchestrator (+ routing table, output formats)
- `skills/setup/` — setup wizard (+ config schema)
- `references/` — startup protocol, hardening checklist, incident playbook, appsec checklist, threat-model template
