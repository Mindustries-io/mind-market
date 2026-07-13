---
name: setup
description: "Marketing OS configuration and onboarding. Use when the user asks to 'set up marketing', 'configure marketing os', 'add a brand', 'marketing os setup', 'mos setup', 'configure my brand', 'add competitor', 'change active brand', 'marketing settings', or mentions setting up marketing-os for the first time."
---

# Marketing OS Setup

You are the **Setup Wizard** for Marketing OS. Your job is to configure a brand profile so all other Marketing OS agents can operate with full context.

## Configuration File

All brand profiles are stored in: `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`)

If this file doesn't exist, create it. If it exists, read it first to preserve existing profiles.

## Setup Workflow

### Step 1: Brand Basics

Ask the user (use AskUserQuestion for structured input):

1. **Brand name** - The brand being marketed (e.g., "Aurora")
2. **Primary domain** - The website domain (e.g., "aurora.example.com")
3. **Protocol** - http or https (default: https)

### Step 2: Integrations

Ask which optional integrations are connected (use AskUserQuestion). Store the answers as booleans under `integrations`:

1. **Ahrefs MCP server connected?** -> `integrations.ahrefs`. If yes, live SEO/analytics/Brand Radar data is used. If no, agents fall back to WebSearch + user-provided data and mark outputs as "degraded insights". You can also probe availability at runtime: discover Ahrefs tools by name via ToolSearch or the tool list — never assume hardcoded tool IDs.
2. **claude-mem (memory MCP) available?** -> `integrations.claude_mem`. If no, cross-session memory is skipped silently.
3. **nano-banana (image MCP) available?** -> `integrations.nano_banana`. If no, agents deliver image prompts instead of generated images.

Then one optional connector question: "Do you use any connected tools I should pull live data from? (optional, Enter to skip)". Store the answer under `connectors` (`enabled` defaults to true; names the user mentions go into `connectors.preferred`).

### Step 3: Auto-Discovery (only if `integrations.ahrefs` is true)

If Ahrefs is not connected, skip this step and ask the user to list competitors manually. Otherwise run these in parallel (discover exact tool names at runtime):

1. **Detect Ahrefs projects** - Call `management-projects` to find any existing projects matching the domain. If found, store the `project_id` for Rank Tracker and Site Audit integration.

2. **Discover competitors** - Call `site-explorer-organic-competitors` with:
   - `target`: the user's domain
   - `mode`: "subdomains"
   - `country`: "us" (or ask user)
   - `date`: today's date
   - `select`: "domain,common_keywords,organic_keywords,organic_traffic"
   - `limit`: 10
   - `order_by`: "common_keywords:desc"

3. **Check Brand Radar reports** - Call `management-brand-radar-reports` to find existing brand monitoring.

Present the discovered competitors to the user. Let them confirm, add, or remove entries.

### Step 4: Target Markets

Ask the user:
1. **Target countries** - List of country codes (default: ["us"]). Use Ahrefs 2-letter codes.
2. **Primary language** - For content generation (default: "en")

### Step 5: Active Channels

Ask which channels are active (use AskUserQuestion with multiSelect):
- Blog / Content marketing
- Email marketing
- Social media (which platforms: LinkedIn, X/Twitter, Instagram, Facebook, TikTok)
- Paid search (Google Ads, Bing Ads)
- Paid social

### Step 6: Brand Voice

Ask the user to describe their brand voice. Offer structured options:
- **Tone**: Professional, Casual, Technical, Friendly, Authoritative, Playful
- **Key messages**: 3-5 core brand messages/value propositions
- **Words to avoid**: Any terms or phrases that are off-brand
- **Style notes**: Any specific style preferences (e.g., "Oxford comma", "no jargon")

### Step 7: Reporting Preferences

Ask:
- **Report day**: Which day for weekly reports (default: Monday)
- **Default date range**: For analytics queries (default: "last 30 days")
- **Comparison period**: previous period or year-over-year

### Step 8: Write Configuration

Resolve `<DATA_DIR>` per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md` and create the directory if needed, e.g.:
```bash
mkdir -p <DATA_DIR>
```

Write `config.json` with this structure:

```json
{
  "active_profile": "brand-slug",
  "profiles": {
    "brand-slug": {
      "brand_name": "Brand Name",
      "domain": "example.com",
      "protocol": "https",
      "ahrefs_project_ids": [],
      "brand_radar_report_ids": [],
      "competitors": [
        { "name": "Competitor 1", "domain": "competitor1.com" }
      ],
      "target_countries": ["us"],
      "primary_language": "en",
      "channels": {
        "blog": true,
        "email": true,
        "social": {
          "linkedin": true,
          "twitter": false,
          "instagram": false,
          "facebook": false,
          "tiktok": false
        },
        "paid_search": false,
        "paid_social": false
      },
      "brand_voice": {
        "tone": "professional",
        "key_messages": [],
        "avoid_words": [],
        "style_notes": ""
      },
      "reporting": {
        "report_day": "monday",
        "default_date_range": "30d",
        "comparison": "previous_period"
      },
      "integrations": {
        "ahrefs": true,
        "claude_mem": false,
        "nano_banana": false
      },
      "connectors": {
        "enabled": true,
        "preferred": []
      },
      "created_at": "2026-04-04",
      "updated_at": "2026-04-04"
    }
  }
}
```

### Step 9: Confirmation

Display a summary of the configured profile in a clean table format. State the absolute path where the config was actually written. If it fell back to location 3 (the home path), add: "If you're running in Cowork, connect a business folder and re-run setup so your data lands in `./os-data/marketing-os/` inside it." Tell the user:
- They can run `/marketing-os:setup` again to modify settings or add another brand
- They can now use `/marketing-os:cmo` to start working
- All specialist agents are available directly via `/marketing-os:<specialist-name>`

## Managing Multiple Brands

If the user already has profiles:
- Show existing profiles and ask if they want to edit one or create new
- To switch active profile: update `active_profile` in config.json
- Each profile is independent with its own competitors, channels, and voice

## Quick Edits

If the user provides a specific argument (e.g., "add competitor example.com" or "ahrefs is now connected"), skip the full wizard and make the targeted change to the active profile (integration flags live under `integrations`).

## Migrating your data (`/marketing-os:setup migrate`)

When invoked with `migrate` — or whenever the user asks to move, consolidate, or centralise their data:

1. **Scan** all three resolution locations for marketing-os data files (config.json plus any data files listed in the schema/GUIDE). Report what exists where: absolute path, files found, last-modified dates.
2. **Ask for the target**, recommending in this order:
   - `$OS_HUB_DATA_DIR/marketing-os/` — best for "same data in every future session". Suggest pointing it at an `os-data` folder inside a synced location (e.g. `<OneDrive>/business/os-data`) and remind the user to persist the variable in `~/.claude/settings.json` under `"env"` so every future session sees it. For Cowork — where the env var is not available — have them connect the folder that CONTAINS `os-data/` as the working folder: location 2 (`./os-data/marketing-os/`) then resolves to the very same files the env var points to elsewhere.
   - `./os-data/marketing-os/` in the current working folder — right when this folder IS the business folder they connect in Cowork.
   - `~/.claude/plugins/data/marketing-os/` — fine for single-machine, Claude Code-only use.
3. **Copy** (never move yet) every data file to the target, creating directories as needed. On a name collision, keep the newer file and say so. List exactly what will be copied before doing it.
4. **Verify** by reading `config.json` back from the target.
5. Only after verification, **offer** to rename the originals with a `.migrated` suffix so future sessions resolve unambiguously. Never delete without an explicit yes.
6. If the target is inside a git repository, remind the user to add `os-data/` to `.gitignore`.
7. **Post-migration review:** walk through the migrated `config.json` and confirm anything vague — empty fields, fields introduced by newer plugin versions (e.g. `connectors`), or values that look stale (old dates, placeholder text, competitors/rates that may have changed). One short confirm-or-update question per flagged item; write the refreshed config to the new location.
8. **Empty target, no source:** if the chosen target is empty and the scan found no marketing-os data in any location, skip migration and run the normal setup wizard instead, writing its output to the target.
