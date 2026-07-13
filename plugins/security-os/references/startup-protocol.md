# Security OS — Shared Startup Protocol

Every Security OS agent and skill follows these steps before doing any task.

## Data directory

`<DATA_DIR>` — the Security OS data directory — resolves in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/security-os/`.
2. Else if `./os-data/security-os/` exists relative to the current working directory (Cowork: the user's connected folder) → use it.
3. Else → `~/.claude/plugins/data/security-os/` (Claude Code default; unchanged for existing users).

- **READ** (config.json, snapshots): use the first location in that order where the file exists (for location 1, "env var set" is enough to select it).
- **WRITE** (setup wizard writing config.json; data files): use the first WRITABLE location in the same order, creating directories as needed. In practice: env var if set, else `./os-data/security-os/` if the CWD is writable and the session is a sandbox/Cowork-style session or `./os-data/` already exists, else the home path. When both 2 and 3 are plausible on WRITE and neither exists yet, prefer 3 (home) in Claude Code and 2 in sandboxed sessions where 3 is unreachable — the practical test is: try in order, first successful write wins.

## 1. Load Configuration

Read `<DATA_DIR>/config.json` (resolved per the Data directory section above) and extract the `active_profile`.

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

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold security-os data (absolute paths + last-modified) and suggest `/security-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/security-os/` has no `config.json`, or `./os-data/` exists but `./os-data/security-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds security-os data, say so and offer `/security-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/security-os:setup` wizard — writing the result to the selected location.
