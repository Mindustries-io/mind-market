# Marketing OS Configuration Schema

## File Location
`~/.claude/plugins/data/marketing-os/config.json`

## Schema

| Field | Type | Required | Description |
|---|---|---|---|
| `active_profile` | string | yes | Slug of the currently active brand profile |
| `profiles` | object | yes | Map of profile slug -> profile object |

### Profile Object

| Field | Type | Required | Default | Description |
|---|---|---|---|---|
| `brand_name` | string | yes | - | Display name of the brand |
| `domain` | string | yes | - | Primary domain (no protocol) |
| `protocol` | string | no | "https" | http or https |
| `ahrefs_project_ids` | number[] | no | [] | Ahrefs project IDs for Rank Tracker / Site Audit |
| `brand_radar_report_ids` | string[] | no | [] | Ahrefs Brand Radar report IDs |
| `competitors` | object[] | no | [] | Array of {name, domain} objects |
| `target_countries` | string[] | no | ["us"] | Ahrefs 2-letter country codes |
| `primary_language` | string | no | "en" | ISO language code |
| `channels` | object | no | - | Active marketing channels (see below) |
| `brand_voice` | object | no | - | Brand voice configuration (see below) |
| `reporting` | object | no | - | Reporting preferences (see below) |
| `integrations` | object | no | - | Optional integration flags (see below) |
| `created_at` | string | no | - | ISO date of profile creation |
| `updated_at` | string | no | - | ISO date of last update |

### Channels Object

| Field | Type | Default |
|---|---|---|
| `blog` | boolean | false |
| `email` | boolean | false |
| `social.linkedin` | boolean | false |
| `social.twitter` | boolean | false |
| `social.instagram` | boolean | false |
| `social.facebook` | boolean | false |
| `social.tiktok` | boolean | false |
| `paid_search` | boolean | false |
| `paid_social` | boolean | false |

### Brand Voice Object

| Field | Type | Default |
|---|---|---|
| `tone` | string | "professional" |
| `key_messages` | string[] | [] |
| `avoid_words` | string[] | [] |
| `style_notes` | string | "" |

### Reporting Object

| Field | Type | Default |
|---|---|---|
| `report_day` | string | "monday" |
| `default_date_range` | string | "30d" |
| `comparison` | string | "previous_period" |

### Integrations Object

All integrations are optional; agents also probe actual tool availability at runtime and fall back gracefully (see `references/startup-protocol.md` at the plugin root).

| Field | Type | Default | When false |
|---|---|---|---|
| `ahrefs` | boolean | false | Agents use WebSearch + user-provided data with a "degraded insights" note |
| `claude_mem` | boolean | false | Cross-session memory steps are skipped silently |
| `nano_banana` | boolean | false | Agents deliver image prompts instead of generated images |

## Ahrefs Country Codes (Common)

us, gb, de, fr, it, es, br, ca, au, in, jp, nl, se, no, dk, fi, pl, at, ch, be, ie, pt, mx, ar, cl, co
