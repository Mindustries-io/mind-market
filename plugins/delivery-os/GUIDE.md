# Delivery OS

Skills for running a services/consulting delivery organization as a measurable system:

- **delivery-scorecard** — build or refresh the department scorecard (CSAT, delivery-margin
  deviation, PoC-to-Production conversion, expansion revenue %, upsells), rolled up across the
  project portfolio with per-account detail and RAG statuses. Use it for the weekly delivery
  review and as the evidence base for exec/board reporting.
- **adp-generator** — generate or update an Account Development Plan per Tier 1 / Tier 2
  account: stakeholder map with Business Value / Personal Value / Trust Value scoring,
  expansion pipeline, next-best actions, and account health.

Both skills work from data you provide (pasted exports, spreadsheets, or files in your working
folder) and never require credentials. When a project-management or CRM connector is available
in the session, the skills may offer to pull live data instead — exports remain the fallback.

Configuration and data resolve per the shared convention: `$OS_HUB_DATA_DIR/delivery-os/` if
that environment variable is set → `./os-data/delivery-os/` when `./os-data/` exists in your
working folder (Cowork: your connected folder or drive) → `~/.claude/plugins/data/delivery-os/`
(Claude Code default). Run `/delivery-os:setup` once to create the profile (org, account tiers,
KPI targets, cadence).

Data note: scorecards and ADPs contain client-confidential information. Keep the data directory
in the folder or drive of the organization the data describes — never mix organizations — and
add `os-data/` to `.gitignore` if your working folder is a git repository.

Unlike the other OSs in this marketplace, delivery-os is skills-only (no orchestrator agent):
both workflows are single-threaded by design and invoked directly with `/delivery-os:delivery-scorecard`
and `/delivery-os:adp-generator`; `/delivery-os:setup` handles configuration and migration.
