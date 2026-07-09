# Security OS — Shared Startup Protocol

Every Security OS agent and skill follows these steps before doing any task.

## 1. Load Configuration

Read `~/.claude/plugins/data/security-os/config.json` and extract the `active_profile`.

**Graceful degradation:** if the file is missing or empty, do NOT hard-stop. Instead:
- Offer a 3-question inline quick-setup: (1) org/product name and domain, (2) tech stack (languages, hosting, cloud/SaaS), (3) crown-jewel data (what would hurt most if leaked/lost). Write the answers to the config.
- Or proceed with whatever context the user already gave, noting reduced personalization.
- Mention that `/security-os:setup` runs the full wizard.

## 2. Memory

If a memory search tool (e.g. the claude-mem MCP server) is available, search `"sec: {ORG}"` for recent security context (open findings, past incidents, vendor assessments, posture scores). If no memory tool is available, proceed silently without it. All Security OS memory writes use the `sec:` prefix.

## 3. Standard Variables

Extract from the active profile:

| Variable | Config field |
|---|---|
| `ORG` | `org_name` |
| `DOMAIN` | `domain` |
| `STACK` | `stack` (languages, frameworks, hosting, cloud, saas_tools) |
| `CROWN_JEWELS` | `crown_jewels` |
| `CONTROLS` | `existing_controls` |
| `RISK_LEVEL` | `risk_appetite.level` |
| `CROSS_OS` | `cross_os` flags (e.g. `legal_os_installed`) |

## 4. External Tools & Fallbacks

Never fail hard on a missing integration:

| Tool | Use | Fallback |
|---|---|---|
| `static-analysis:semgrep` / `static-analysis:codeql` skills | Automated code scanning | Manual review playbook in `references/appsec-review-checklist.md` |
| `npm audit` / `pip-audit` / `osv-scanner` (CLI) | Dependency vulnerability check | Ask the user to paste lockfile/manifest; check advisories via WebSearch |
| WebSearch / WebFetch | Breach history, advisories, vendor trust pages | Ask the user to paste relevant material; note the gap |
| legal-os plugin | GDPR/DPA side of incidents and vendors | Include a minimal notification-obligations reminder; suggest professional counsel |
