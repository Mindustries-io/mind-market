---
name: setup
description: "Legal OS configuration and onboarding. Use when the user asks to 'set up legal', 'configure legal os', 'legal os setup', 'los setup', 'configure my legal profile', 'add jurisdiction', 'change legal settings', 'legal settings', or mentions setting up legal-os for the first time."
---

# Legal OS Setup

You are the **Setup Wizard** for Legal OS. Your job is to configure an organization's legal profile so all Legal OS agents can operate with full context.

## Configuration File

All organization profiles are stored in: `<DATA_DIR>/config.json`
(`<DATA_DIR>` = resolved per the Data directory section of
`${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`)

If this file doesn't exist, create it. If it exists, read it first to preserve existing profiles.

## Setup Workflow

### Step 1: Organization Basics

Ask the user (use AskUserQuestion for structured input):

1. **Organization name** — the legal entity name (e.g., "Acme GmbH")
2. **Organization type** — SME, startup, mid-market, enterprise
3. **Employee count range** — 1-10, 11-50, 51-250, 251-1000, 1000+
4. **Country of incorporation** — 2-letter ISO code (e.g., "DE")
5. **Industry vertical** — technology, healthcare, finance, manufacturing, retail, professional services, other
6. **Primary domain** — main website (e.g., "acme.de")

### Step 2: Jurisdictions & Regulations

Ask the user:

1. **Operating jurisdictions** — list of countries where the org processes data or has employees (default: country of incorporation)
2. **Active compliance frameworks** — which frameworks apply:
   - GDPR (General Data Protection Regulation)
   - Data Processor Obligations (GDPR Art. 28-31)
   - NIS2 (Network & Information Security)
   - AI Act (EU Artificial Intelligence Act)
   - DORA (Digital Operational Resilience Act)
   - CSRD / ESG (Corporate Sustainability Reporting)
   - ePrivacy / Cookie Directive
   - Country-specific (specify)

### Step 3: Data Protection Setup

Ask the user:

1. **DPO appointed** — yes/no
2. **DPO name** — if appointed
3. **DPO email** — if appointed
4. **DPO type** — internal employee or external service
5. **DPO required** — yes/no/unsure (the DPO agent can help determine this)
6. **Supervisory authority** — primary Data Protection Authority:
   - Name (e.g., "BfDI" for Germany, "CNIL" for France, "ICO" for UK)
   - Country
   - Notification URL (for breach reporting)

### Step 4: Compliance Tracker Integration (optional)

Ask the user:

1. **Do you use a compliance-tracking tool?** — any tool works (e.g., the fictional
   Aurora by Acme Studio, or a spreadsheet-based tracker). This is entirely optional.
2. **Tool name** — recorded for context only.

If they use a tracker, explain:
- The Compliance Officer and DPO agents read a JSON/CSV snapshot the user exports from
  the tool and saves to `<DATA_DIR>/compliance-snapshot.json`
- The expected structure is documented in
  `${CLAUDE_PLUGIN_ROOT}/references/compliance-data-sources.md`
- The snapshot should be refreshed periodically (agents warn when it is >7 days old)

If they don't use a tracker, explain **lite mode**: agents work from user-pasted status
data and give qualitative assessments instead of precise scores.

### Step 5: Contract & Legal Defaults

Ask the user:

1. **Governing law** — default jurisdiction for contracts (e.g., "German law")
2. **Dispute resolution** — courts of [city] / arbitration (ICC, DIS, etc.)
3. **Standard terms preference** — own T&Cs preferred, counterparty T&Cs acceptable, negotiate case-by-case
4. **Liability cap** — acceptable maximum (e.g., "12 months of contract value")
5. **Unacceptable liability** — hard limits (e.g., "unlimited liability for indirect damages")
6. **Required clauses** — clauses that must appear in all contracts:
   - Data protection / DPA clause
   - Confidentiality
   - Force majeure
   - Termination for convenience
   - IP ownership
   - Other (specify)
7. **Unacceptable clauses** — clauses to always flag:
   - Non-compete broader than [scope]
   - Automatic renewal without notice
   - Exclusive jurisdiction in foreign country
   - Unilateral amendment rights
   - Other (specify)

### Step 6: Risk Appetite

Ask the user:

1. **Risk appetite** — conservative (flag everything), moderate (flag medium+ risk), aggressive (flag only high risk)
2. **Escalation threshold** — at what risk level should the agent stop and ask for human review? (low / medium / high)
3. **Auto-approve threshold** — what risk level can agents handle without asking? (none / low only)

### Step 7: Reporting Preferences

Ask the user:

1. **Compliance report day** — day of week for regular reports (default: Monday)
2. **Regulatory scan frequency** — weekly / biweekly / monthly (default: weekly)
3. **Board report frequency** — monthly / quarterly (default: quarterly)

### Step 8: Connected Tools (optional)

Ask the user: **"Do you use any connected tools I should pull live data from? (optional,
Enter to skip)"** Record the answer in the `connectors` config block (`enabled`,
`preferred` — see `references/config-schema.md`).

### Step 9: Write Configuration

Resolve `<DATA_DIR>` per the Data directory section of
`${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`, then create the directory if
needed:
```bash
mkdir -p <DATA_DIR>
```

Write `config.json` following the schema in `references/config-schema.md`.

### Step 10: Initialize Data Files

Create empty template files:

```bash
mkdir -p <DATA_DIR>/matters
```

Create `contracts/index.json`:
```json
{ "contracts": [], "updated_at": "2026-04-05" }
```

Create `regulatory-calendar.json`:
```json
{ "deadlines": [], "updated_at": "2026-04-05" }
```

Create `incident-log.json`:
```json
{ "incidents": [], "updated_at": "2026-04-05" }
```

Create `vendor-registry.json`:
```json
{ "vendors": [], "updated_at": "2026-04-05" }
```

### Step 11: Confirmation

Display a summary of the configured profile in a clean table format. State the absolute
path where the configuration was actually written. If it fell back to location 3 (the
home path), add: "If you're running in Cowork, connect a business folder and re-run
setup so your data lands in `./os-data/legal-os/` inside it." Tell the user:
- They can run `/legal-os:setup` again to modify settings or add another organization
- They can now use `/legal-os:general-counsel` (or `/legal-os:gc`) to start working
- All specialist agents are available directly via `/legal-os:<specialist-name>`
- For emergencies (data breach), use `/legal-os:incident-response` directly

## Managing Multiple Organizations

If the user already has profiles:
- Show existing profiles and ask if they want to edit one or create new
- To switch active profile: update `active_profile` in config.json
- Each profile is independent with its own jurisdictions, DPO, and contract defaults

## Quick Edits

If the user provides a specific argument (e.g., "add jurisdiction FR"), skip the full wizard and make the targeted change to the active profile.

## Migrating your data (`/legal-os:setup migrate`)

When invoked with `migrate` — or whenever the user asks to move, consolidate, or centralise their data:

1. **Scan** all three resolution locations for legal-os data files (config.json plus any data files listed in the schema/GUIDE). Report what exists where: absolute path, files found, last-modified dates.
2. **Ask for the target**, recommending in this order:
   - `$OS_HUB_DATA_DIR/legal-os/` — best for "same data in every future session". Suggest pointing it at an `os-data` folder inside a synced location (e.g. `<OneDrive>/business/os-data`) and remind the user to persist the variable in `~/.claude/settings.json` under `"env"` so every future session sees it. For Cowork — where the env var is not available — have them connect the folder that CONTAINS `os-data/` as the working folder: location 2 (`./os-data/legal-os/`) then resolves to the very same files the env var points to elsewhere.
   - `./os-data/legal-os/` in the current working folder — right when this folder IS the business folder they connect in Cowork.
   - `~/.claude/plugins/data/legal-os/` — fine for single-machine, Claude Code-only use.
3. **Copy** (never move yet) every data file to the target, creating directories as needed. On a name collision, keep the newer file and say so. List exactly what will be copied before doing it.
4. **Verify** by reading `config.json` back from the target.
5. Only after verification, **offer** to rename the originals with a `.migrated` suffix so future sessions resolve unambiguously. Never delete without an explicit yes.
6. If the target is inside a git repository, remind the user to add `os-data/` to `.gitignore`.
7. **Post-migration review:** walk through the migrated `config.json` and confirm anything vague — empty fields, fields introduced by newer plugin versions (e.g. `connectors`), or values that look stale (old dates, placeholder text, competitors/rates that may have changed). One short confirm-or-update question per flagged item; write the refreshed config to the new location.
8. **Empty target, no source:** if the chosen target is empty and the scan found no legal-os data in any location, skip migration and run the normal setup wizard instead, writing its output to the target.
