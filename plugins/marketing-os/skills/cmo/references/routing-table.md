# CMO Routing Table

Complete mapping of user intents to Marketing OS specialists.

## Route: seo-strategist

**Triggers:** keyword research, keyword analysis, keyword opportunities, organic keywords, ranking, rankings, SERP, search results, backlinks, backlink audit, backlink profile, referring domains, link building, technical SEO, site audit, site health, crawl errors, on-page SEO, meta tags, title tags, page speed, core web vitals, indexing, sitemap, robots.txt, canonical, internal linking, content gaps, keyword gaps, search volume, keyword difficulty, CPC

**Example prompts:**
- "What keywords should I target?"
- "Run an SEO audit on my site"
- "Find backlink opportunities"
- "What's my domain rating?"
- "Show me my top organic keywords"
- "Find content gaps vs competitors"
- "What are the easiest keywords to rank for?"
- "Analyze the SERP for [keyword]"
- "Check my site's technical health"
- "How many referring domains do I have?"

**Secondary:** analytics-reporter (for traffic context)

## Route: content-director

**Triggers:** blog post, article, write, draft, content, content calendar, editorial, headline, copy, copywriting, landing page, press release, case study, whitepaper, content brief, content strategy, topic clusters, pillar page, content plan, blog ideas

**Example prompts:**
- "Write a blog post about [topic]"
- "Create a content calendar for next month"
- "Draft a landing page for [product]"
- "Give me 10 blog post ideas"
- "Create a content brief for [topic]"
- "Write a press release for [announcement]"
- "Plan my content strategy for Q2"

**Secondary:** seo-strategist (for keyword data)

## Route: competitive-intel

**Triggers:** competitor, competitors, competitive, compete, market share, positioning, battlecard, market research, competitive landscape, competitor analysis, competitor keywords, competitor traffic, competitor backlinks, content gap

**Example prompts:**
- "Who are my main competitors?"
- "How do I compare to [competitor]?"
- "What keywords do competitors rank for that I don't?"
- "Create a competitive battlecard"
- "Analyze [competitor]'s content strategy"
- "What's the competitive landscape in my niche?"
- "Find competitor backlink opportunities"

**Secondary:** seo-strategist (for keyword overlap data)

## Route: email-campaigns

**Triggers:** email, email sequence, drip, newsletter, welcome series, onboarding, nurture, re-engagement, win-back, cart abandonment, email copy, subject line, email marketing, email campaign, autoresponder, email flow

**Example prompts:**
- "Design a welcome email sequence"
- "Write a newsletter about [topic]"
- "Create a re-engagement campaign"
- "Draft a product launch email series"
- "Write subject lines for [campaign]"
- "Plan my email marketing for the month"

**Secondary:** content-director (for copy quality)

## Route: social-media

**Triggers:** social, social media, post, tweet, LinkedIn, Instagram, Facebook, TikTok, hashtag, social calendar, social strategy, engagement, followers, social content, reels, stories, social ads

**Example prompts:**
- "Write a LinkedIn post about [topic]"
- "Create a social media calendar"
- "What hashtags should I use for [topic]?"
- "Plan my social strategy for [product launch]"
- "Write 5 tweets about [topic]"
- "Create an Instagram caption for [image]"

**Secondary:** content-director (for messaging alignment)

## Route: analytics-reporter

**Triggers:** report, analytics, traffic, performance, dashboard, KPI, metrics, conversion, bounce rate, sessions, visitors, pageviews, organic traffic, paid traffic, web analytics, weekly report, monthly report, quarterly review, ROI, attribution

**Example prompts:**
- "Give me a weekly performance report"
- "How is my organic traffic trending?"
- "Show me my top performing pages"
- "Create a marketing dashboard"
- "What's my traffic by country?"
- "Compare this month to last month"
- "What are my most important KPIs?"

**Secondary:** seo-strategist (for search-specific metrics)

## Route: brand-guardian

**Triggers:** brand, brand voice, AI visibility, share of voice, brand mentions, brand monitoring, AI search, ChatGPT, AI overview, brand radar, brand consistency, voice check, tone, messaging, AI citations, LLM mentions

**Example prompts:**
- "How visible is my brand in AI search?"
- "What's my share of voice vs competitors?"
- "Check this content against our brand voice"
- "Which of my pages get cited by AI?"
- "Monitor brand mentions in AI responses"
- "How is our AI visibility trending?"

**Secondary:** competitive-intel (for competitor SOV comparison)

## Route: Full Pipeline (CMO orchestrates all)

**Triggers:** campaign, launch, marketing plan, go-to-market, GTM, full marketing strategy, comprehensive plan, marketing playbook, annual plan, quarterly plan

**Example prompts:**
- "Plan a product launch campaign"
- "Create a comprehensive marketing strategy"
- "Build a go-to-market plan for [product]"
- "What should our marketing plan be for Q3?"
- "Design a full campaign for [initiative]"

**Pipeline order:** competitive-intel -> seo-strategist -> content-director -> email-campaigns -> social-media -> analytics-reporter -> brand-guardian

## Ambiguous Requests

If the request is ambiguous, use these heuristics:
1. If it mentions a website or domain -> seo-strategist
2. If it mentions writing or creating -> content-director
3. If it mentions numbers or data -> analytics-reporter
4. If it mentions competitors by name -> competitive-intel
5. If it mentions a channel (email, social) -> that channel's specialist
6. If truly unclear -> ask the user to clarify
