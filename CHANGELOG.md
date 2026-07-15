# Changelog

All notable changes to the Mindustries OS Hub marketplace and its plugins.
The format follows [Keep a Changelog](https://keepachangelog.com/); versions refer to the marketplace manifest.

## [2.1.0] — 2026-07-13

Portable data, live connectors, and a safe upgrade/migration path (PR #1). Plugin versions: legal-os / marketing-os / po-os → 1.2.0; security-os / finance-os / sales-os / support-os → 1.1.0.

### Added
- **Portable data directory resolution** — every OS resolves its data directory as: `$OS_HUB_DATA_DIR/<os>/` (env var) → `./os-data/<os>/` (selected when `./os-data/` exists in the working folder — the Cowork path) → `~/.claude/plugins/data/<os>/` (Claude Code default, unchanged for existing users). Documented once per plugin in `references/startup-protocol.md` and referenced everywhere else as `<DATA_DIR>`. One WRITE algorithm with ordered fallback and a session-only last resort; a deliberately selected but empty location triggers a migration offer or a fresh setup wizard instead of silently falling back.
- **`/os:setup migrate`** — scans all three locations, copies data to a chosen target, verifies, walks through a post-migration review of stale or incomplete fields, and only then offers to retire the originals (`.migrated` rename, never silent deletion). Startup warns when multiple locations hold data.
- **Optional MCP connector awareness** (exports/paste remain the default): finance-os (bookkeeper, cashflow-forecaster, invoicing — e.g. Qonto, Stripe), sales-os (pipeline-manager, crm-hygienist — e.g. HubSpot, Pipedrive, Attio), and support-os (ticket-triage, insights-analyst — e.g. Zendesk, Intercom) discover connectors at runtime by name and offer read-only live pulls. New `connectors: { enabled, preferred }` block in all seven config schemas with one optional setup question; never credentials, never required.
- **Upgrade-path guarantee** — configs from older plugin versions stay valid: missing fields take documented defaults (a missing `connectors` block means enabled), and no setup re-run is ever required after a plugin update.
- README sections: "Where your data lives", centralising data across sessions via a synced folder, and "Live data when connected, exports otherwise".

### Changed
- Setup wizards state the absolute path they actually wrote to and guide Cowork users to create `os-data/` in a connected folder.
- Cross-OS config lookups (e.g. support-os reading marketing-os brand voice) resolve the sibling's data directory with the sibling's own resolution order, read the selected location first, and announce any legacy-data fallback; brand-voice reuse honors the `voice.use_marketing_os_voice` opt-in.
- sales-os `pipeline.file_path` documented as supporting a literal `<DATA_DIR>` placeholder token, substituted at read/write time so the path survives migrations.
- Brand scrub in plugin prose: marketplace/owner names appear only in README and manifests; proxied-backend doc links are now relative and fork-agnostic; per-OS `.gitignore` warnings use OS-appropriate data examples.

### Fixed
- 74 review findings across 12 Copilot review rounds, including: contradictory READ/WRITE resolution wording, the `<DATA_DIR>/../` sibling-path bug, a connectors boolean/list mix-up in the po-os wizard, stale "never connects to APIs" absolutes conflicting with connector awareness, and unconditional CRM reads bypassing the `connectors.enabled` opt-out.

## [2.0.0] — 2026-07-09

Marketplace restructure: agent-based architecture, four new OSs, publication hygiene.

### Added
- **Four new Operating Systems** (v1.0.0): **security-os** (vCISO: appsec-reviewer, threat-modeler, hardening-auditor, vendor-security, security-incident — with a mandatory GDPR handoff to legal-os when personal data is affected), **finance-os** (CFO: bookkeeper, invoicing, cashflow-forecaster, tax-planner, pricing-analyst), **sales-os** (Sales Director: pipeline-manager, outreach-writer, proposal-drafter, crm-hygienist), and **support-os** (Support Lead: ticket-triage, response-drafter, kb-writer, insights-analyst feeding po-os discovery).
- **Agent architecture with model/effort tiering** — specialists moved from skills into `agents/*.md` declaring `model` and `effort` in frontmatter: `haiku`/low for mechanical work, `sonnet`/medium for playbook-driven drafting, `inherit`/high for judgment-heavy work (portable to proxied backends). Thin trigger skills keep every `/os:specialist` slash command working.
- **Fast mode** in every orchestrator: single-task requests go to exactly one specialist; pipelines only for genuinely multi-domain work, announced before running.
- Shared per-plugin startup protocol (config load with graceful 3-question quick-setup, optional memory, tool fallbacks) replacing ~90% duplicated boilerplate; lazy, conditional loading of reference files.
- Root `README.md` (including a step-by-step proxied-backend/Ollama setup guide), `LICENSE` (MIT), `.gitignore`, `SAFETY.md` disclaimers (legal-os, po-os, security-os, finance-os, sales-os), and a CI workflow running `claude plugin validate --strict` on every push and PR.

### Changed
- Marketplace manifest moved to the required `.claude-plugin/marketplace.json` path (previously in a `.claude-plugins/` directory that Claude Code would not recognize) and enriched with display names, keywords, license, and repository metadata. Existing plugins bumped to 1.1.0.
- Author metadata unified as Mindustries; graceful degradation added for optional integrations (no-Ahrefs mode in marketing-os, generic compliance-tracker export with lite mode in legal-os, optional memory everywhere).

### Removed
- Personal and product references (author's name, ObligoBoard examples, personal GitHub handles) replaced with the fictional Aurora / Acme Studio examples.
- Hardcoded MCP server UUIDs and session-specific tool IDs, replaced with product-name references and runtime tool discovery.

## [1.0.0] — 2026-07-09

Initial release.

### Added
- Marketplace manifest (`mindustries-os-hub`) with three skill-based plugins:
  - **marketing-os** — CMO orchestrator with specialists for SEO, content, competitive intelligence, email campaigns, social media, analytics, and brand monitoring.
  - **legal-os** — General Counsel orchestrator with specialists for compliance, GDPR/data protection, contracts, legal research, regulatory intelligence, incident response, and vendor risk.
  - **po-os** — Lead PO orchestrator with specialists for regulatory-driven features, jurisdictional localization, and voice-of-customer discovery.
- Skill-based delegation (orchestrator invoking specialist skills), per-plugin setup wizards writing to `~/.claude/plugins/data/<os>/config.json`, per-specialist reference libraries, and cross-OS memory namespaces (`mos:`, `los:`, `po:`).

## 2.2.0 — 2026-07-15

- New plugin: **delivery-os 1.0.0** — delivery-scorecard and adp-generator skills for
  services delivery leaders (skills-only plugin, no orchestrator).
