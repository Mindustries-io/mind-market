# Delivery OS — Shared Startup Protocol

Both Delivery OS skills follow these steps before doing any task.

## Data Directory

`<DATA_DIR>` — the Delivery OS data directory — resolves in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/delivery-os/`.
2. Else if `./os-data/` exists relative to the current working directory (Cowork: the user's connected folder or drive) → use `./os-data/delivery-os/` inside it, creating the subdirectory if needed.
3. Else → `~/.claude/plugins/data/delivery-os/` (Claude Code default).

- **READ** (config.json, scorecard.json, adp/*.md): select the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists, else 3 — and read from it. If a deliberately selected location (1 or 2) contains no `config.json`, do NOT silently fall back to a lower-priority location: say what was found where and offer `/delivery-os:setup` (or a migration if data exists elsewhere).
- **WRITE** (setup writing config.json; scorecard snapshots; ADP files): write to the selected location, creating directories as needed. If it is not writable, try the remaining locations in priority order and tell the user where the file actually landed. If nothing is writable, keep the output in the session for the user to save manually and say that persistence was skipped.

## Load Configuration

Read `<DATA_DIR>/config.json`. If missing, do NOT hard-stop: offer a 3-question inline quick-setup — (1) organization name, (2) account tiers in use (e.g. Tier 1 / Tier 2 definitions), (3) KPI targets to track against (or accept the skill defaults) — write the answers to the config, then continue. Mention that `/delivery-os:setup` runs the full wizard.

## Data Sensitivity

Scorecards and ADPs contain client-confidential information. `<DATA_DIR>` should live in the folder or drive that belongs to the organization the data describes — never mix two organizations' delivery data in one data directory. If the working folder is a git repository, `os-data/` belongs in `.gitignore`.
