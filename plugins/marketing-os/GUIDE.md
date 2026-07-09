# Marketing OS — User Guide

A full-stack marketing operating system for Claude Code. One orchestrator (the CMO), seven specialist agents, all driven from your CLI.

---

## Quick Start

### 1. Set Up Your First Brand

```
/marketing-os:setup
```

The setup wizard walks you through:

- **Brand name** and **domain** (e.g., "Acme Corp" / acme.com)
- **Competitors** — auto-discovered from Ahrefs organic overlap, you confirm the list
- **Target countries** — where your audience lives (default: US)
- **Active channels** — blog, email, social (which platforms), paid
- **Brand voice** — tone, key messages, words to avoid
- **Reporting preferences** — weekly report day, default date range

Configuration is saved to `~/.claude/plugins/data/marketing-os/config.json`. You can manage multiple brands and switch between them.

### 2. Talk to the CMO

```
/marketing-os:cmo [your request]
```

The CMO is your main entry point. Describe what you need in plain language — it routes to the right specialist(s) automatically.

**Examples:**

```
/marketing-os:cmo What are my best keyword opportunities?
/marketing-os:cmo Write a blog post about cloud migration
/marketing-os:cmo How do I compare to competitors?
/marketing-os:cmo Create a content calendar for May
/marketing-os:cmo Design a welcome email sequence
/marketing-os:cmo How visible is my brand in AI search?
/marketing-os:cmo Give me a weekly performance report
/marketing-os:cmo Plan a product launch campaign for [product]
```

### 3. Go Direct to a Specialist

Skip the CMO when you know exactly which agent you need:

```
/marketing-os:seo-strategist Find easy keyword wins (positions 4-20)
/marketing-os:content-director Write a landing page for [product]
/marketing-os:competitive-intel Build a battlecard for [competitor]
/marketing-os:email-campaigns Design a cart abandonment sequence
/marketing-os:social-media Write a LinkedIn post about [topic]
/marketing-os:analytics-reporter Monthly performance report
/marketing-os:brand-guardian Check my AI share of voice
```

---

## The Team

| Agent | Domain | When to Use |
|---|---|---|
| **CMO** | Orchestration | You're not sure who to ask, or need multiple specialists |
| **SEO Strategist** | Organic search | Keywords, backlinks, audits, SERP analysis, technical SEO |
| **Content Director** | Content creation | Blog posts, landing pages, calendars, briefs |
| **Competitive Intel** | Market analysis | Competitor research, gaps, battlecards, positioning |
| **Email Campaigns** | Email marketing | Sequences, newsletters, drip flows, subject lines |
| **Social Media** | Social channels | Platform-specific posts, calendars, hashtags |
| **Analytics Reporter** | Performance data | Traffic reports, dashboards, KPI tracking |
| **Brand Guardian** | Brand health | AI visibility, share of voice, voice consistency |

---

## How Conversations Work

### Single-specialist tasks

You ask something, the CMO sends it to one agent, you get a structured answer.

```
You:   "What keywords should I target for cloud security?"
CMO:   Routes to SEO Strategist
       -> Pulls keyword data from Ahrefs
       -> Returns: keyword table with volume, difficulty, CPC, opportunities
```

### Multi-specialist tasks

The CMO coordinates two or more agents, then synthesizes.

```
You:   "Create a content calendar for next month"
CMO:   1. SEO Strategist -> keyword research for your niche
       2. Content Director -> builds calendar using keyword data
       3. Social Media -> adds distribution plan
       -> Returns: unified weekly calendar with topics, keywords, channels
```

### Full pipeline (campaign planning)

The CMO runs all seven specialists in sequence for comprehensive plans.

```
You:   "Plan a product launch campaign for [product]"
CMO:   1. Competitive Intel -> market landscape
       2. SEO Strategist -> keyword opportunities
       3. Content Director -> content strategy + briefs
       4. Email Campaigns -> launch email sequence
       5. Social Media -> social campaign plan
       6. Analytics Reporter -> KPI framework
       7. Brand Guardian -> voice consistency check
       -> Returns: complete campaign plan
```

---

## Tips for Better Results

### Be specific about what you want

| Instead of | Try |
|---|---|
| "Do SEO" | "Find keywords I can rank for with KD under 30 and volume over 500" |
| "Write content" | "Write a 2000-word how-to guide targeting [keyword]" |
| "Check competitors" | "Compare my backlink profile to [competitor.com]" |
| "Email campaign" | "Design a 5-email welcome sequence for SaaS trial signups" |

### Mention the output format you want

- "Give me a table of..." — structured comparison
- "Write a full draft..." — complete content piece
- "Quick summary of..." — executive overview
- "Create a brief for..." — document a writer can execute
- "Build a calendar..." — week-by-week plan

### Reference competitors by name

The system knows your configured competitors, but you can also analyze any domain on the fly:

```
/marketing-os:competitive-intel Analyze hubspot.com vs salesforce.com
```

### Chain requests across sessions

Marketing OS stores findings in memory (claude-mem). Reference past work naturally:

```
Session 1: /marketing-os:cmo Find my top keyword opportunities
Session 2: /marketing-os:cmo Based on the keyword research, create a content calendar
Session 3: /marketing-os:cmo Write the first blog post from the calendar
```

---

## Managing Multiple Brands

### Add a second brand

```
/marketing-os:setup NewBrand
```

### Switch between brands

```
/marketing-os:setup switch to [brand-name]
```

Or edit `~/.claude/plugins/data/marketing-os/config.json` and change the `active_profile` value.

### Quick config changes

```
/marketing-os:setup add competitor example.com
/marketing-os:setup change tone to casual
```

---

## What Data Powers This

Marketing OS pulls real data from your connected integrations:

| Source | What It Provides |
|---|---|
| **Ahrefs Site Explorer** | Domain rating, organic traffic, keywords, backlinks, top pages |
| **Ahrefs Keywords Explorer** | Search volume, keyword difficulty, CPC, related terms |
| **Ahrefs Site Audit** | Technical SEO issues, health scores |
| **Ahrefs Rank Tracker** | Keyword position monitoring over time |
| **Ahrefs Brand Radar** | AI visibility, share of voice, brand mentions in LLMs |
| **Ahrefs Web Analytics** | Visitor stats, traffic sources, top pages |
| **Google Search Console** (via Ahrefs) | Clicks, impressions, CTR, position data |
| **Apify** | Web scraping for competitor content, social data |
| **Web Search** | Current trends, news, supplementary research |
| **nano-banana** | AI-generated images for content and social |
| **claude-mem** | Cross-session memory for trend tracking |

All data is live — every query hits the APIs with current information.

---

## Common Workflows

### Weekly Marketing Routine

**Monday** — Get your bearings:
```
/marketing-os:cmo Weekly performance report
```

**Tuesday** — Plan content:
```
/marketing-os:cmo What should I write about this week? Check trending topics and keyword gaps.
```

**Wednesday** — Check competitors:
```
/marketing-os:cmo Any notable competitor moves this week?
```

**Thursday** — Create content:
```
/marketing-os:content-director Write [this week's blog post]
/marketing-os:social-media Create social posts to promote it
```

**Friday** — Brand check:
```
/marketing-os:brand-guardian How's our AI visibility trending?
```

### New Website Launch

```
1. /marketing-os:setup                              (configure brand)
2. /marketing-os:seo-strategist Full site audit      (baseline health)
3. /marketing-os:competitive-intel Market landscape   (who you're up against)
4. /marketing-os:cmo 90-day marketing plan           (full strategy)
```

### Content Sprint

```
1. /marketing-os:seo-strategist Top 20 keyword opportunities
2. /marketing-os:content-director Content calendar from these keywords
3. /marketing-os:content-director Write brief for [article 1]
4. /marketing-os:content-director Draft [article 1]
5. /marketing-os:brand-guardian Review the draft against our voice
6. /marketing-os:social-media Create social posts for [article 1]
7. /marketing-os:email-campaigns Write newsletter featuring [article 1]
```

---

## Scheduled Automations

Ask the CMO to set up recurring reports:

```
/marketing-os:cmo Set up automated weekly reports and daily SEO monitoring
```

This creates scheduled tasks that run automatically:

| Task | Schedule | What It Does |
|---|---|---|
| Daily SEO check | Weekdays, morning | Flags metric drops over 10% |
| Weekly report | Monday morning | Full performance dashboard |
| Competitor watch | Wednesday | Competitor metrics snapshot |
| Brand radar | Friday | AI visibility and SOV update |
| Monthly audit | 1st of month | Comprehensive deep dive |

Reports are stored in memory, so you can ask about trends:

```
/marketing-os:cmo How has our organic traffic trended over the last month?
```

---

## Troubleshooting

**"Config not found"** — Run `/marketing-os:setup` first.

**"No data returned"** — Check your Ahrefs API quota:
```
/marketing-os:cmo Check my Ahrefs API usage
```

**Wrong brand context** — Verify the active profile:
```
/marketing-os:setup show current config
```

**Want to edit config manually** — The file is at:
```
~/.claude/plugins/data/marketing-os/config.json
```
