---
name: setup
description: "Security OS configuration and onboarding. Use when the user asks to 'set up security', 'configure security os', 'security os setup', 'sos setup', 'configure my security profile', 'update my stack inventory', 'change crown jewels', 'security settings', or mentions setting up security-os for the first time."
---

# Security OS Setup Wizard

Configure the Security OS profile stored at `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`). Full schema: `${CLAUDE_PLUGIN_ROOT}/skills/setup/references/config-schema.md` (read it before writing the file).

## Flow

Ask in conversational batches (2-3 questions at a time), not a 20-question form. Accept "skip" for anything — every field except `org_name` has a sensible default.

### 1. Org profile
- Org/product name, primary domain, what the product does (one line), solo or small team?

### 2. Stack inventory
- Languages/frameworks (e.g. TypeScript/Next.js, Python/Django)
- Hosting/cloud (e.g. Vercel, AWS, Hetzner) and where production data lives
- Code hosting & CI (e.g. GitHub + Actions)
- Key SaaS tools with access to code, data, or money (email provider, payment processor, analytics, AI tools)

### 3. Crown jewels
- "What data or system would hurt most if leaked, tampered with, or lost?" — capture 1-5 items (e.g. customer PII, production DB, Stripe account, source code, the founder's email account).

### 4. Existing controls
- MFA/passkeys where? Password manager? Backups (and ever restore-tested)? Secrets manager or `.env` files? Any monitoring/alerts? Capture free-form; the hardening-auditor verifies later.

### 5. Risk appetite & cross-OS
- Risk appetite: conservative / moderate / aggressive (default: moderate).
- Detect sibling plugins: check whether `legal-os` skills are available (e.g. the skill list contains `legal-os:general-counsel`); set `cross_os.legal_os_installed` accordingly, and ask rather than guess if unsure.

### 6. Connectors (optional)
- "Do you use any connected tools I should pull live data from? (optional, Enter to skip)" — store any names in `connectors.preferred`; `connectors.enabled` defaults to true (set false only if the user wants exports-only).

## Writing the config

1. Read the existing config first (if any) and merge — never clobber other profiles.
2. Resolve the write location per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`, create the directory if needed, write JSON matching the schema, set `created_at`/`updated_at` (ISO dates), set `active_profile`.
3. Show the user the resulting JSON and confirm, stating the absolute path actually used. If the config fell back to location 3 (home), add: "If you're running in Cowork, connect a business folder and re-run setup so your data lands in `./os-data/security-os/` inside it."

## After setup

Suggest the natural first step: "Run a posture check (`hardening-auditor`) to get your baseline score, or ask the `ciso` for a full security health check."

Updates ("change my stack", "add a crown jewel") follow the same flow scoped to the field in question.

## Migrating your data (`/security-os:setup migrate`)

When invoked with `migrate` — or whenever the user asks to move, consolidate, or centralise their data:

1. **Scan** all three resolution locations for security-os data files (config.json plus any data files listed in the schema/GUIDE). Report what exists where: absolute path, files found, last-modified dates.
2. **Ask for the target**, recommending in this order:
   - `$OS_HUB_DATA_DIR/security-os/` — best for "same data in every future session". Suggest pointing it at a synced folder (OneDrive, Dropbox, etc.) and remind the user to persist the variable in Claude Code `settings.json` under `"env"` so every future session sees it. In Cowork, connect that same synced folder and the `./os-data/` path resolves to the same data.
   - `./os-data/security-os/` in the current working folder — right when this folder IS the business folder they connect in Cowork.
   - `~/.claude/plugins/data/security-os/` — fine for single-machine, Claude Code-only use.
3. **Copy** (never move yet) every data file to the target, creating directories as needed. On a name collision, keep the newer file and say so. List exactly what will be copied before doing it.
4. **Verify** by reading `config.json` back from the target.
5. Only after verification, **offer** to rename the originals with a `.migrated` suffix so future sessions resolve unambiguously. Never delete without an explicit yes.
6. If the target is inside a git repository, remind the user to add `os-data/` to `.gitignore`.
7. **Post-migration review:** walk through the migrated `config.json` and confirm anything vague — empty fields, fields introduced by newer plugin versions (e.g. `connectors`), or values that look stale (old dates, placeholder text, competitors/rates that may have changed). One short confirm-or-update question per flagged item; write the refreshed config to the new location.
8. **Empty target, no source:** if the chosen target is empty and the scan found no security-os data in any location, skip migration and run the normal setup wizard instead, writing its output to the target.
