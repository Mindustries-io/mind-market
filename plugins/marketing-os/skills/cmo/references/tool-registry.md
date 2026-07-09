# Marketing OS Tool Registry

Complete catalog of available MCP tools organized by marketing domain. All tools are globally available — no additional configuration needed.

## IMPORTANT: Ahrefs API Rules

1. **Always use `mode=subdomains`** when analyzing a domain name. Using `mode=domain` excludes www and other subdomains.
2. **Always check the `doc` tool first** for the real input schema before calling complex tools: `mcp__73e19d61-74ed-4b6a-b8c2-34ead670bb6c__doc({ tool: "tool-name" })`
3. **Date format:** YYYY-MM-DD (e.g., "2026-04-04")
4. **Country codes:** 2-letter lowercase (e.g., "us", "gb", "it")

---

## SEO & Organic Search

### Site Metrics & Health
| Tool | Purpose |
|---|---|
| `site-explorer-metrics` | Overall SEO metrics (DR, organic traffic, keywords, backlinks) |
| `site-explorer-domain-rating` | Domain Rating for a specific date |
| `site-explorer-domain-rating-history` | DR trend over time |
| `site-explorer-url-rating-history` | URL Rating trend |
| `site-explorer-metrics-history` | Historical traffic/keyword metrics |
| `site-explorer-metrics-by-country` | Metrics broken down by country |

### Keywords
| Tool | Purpose |
|---|---|
| `site-explorer-organic-keywords` | Keywords a domain ranks for |
| `site-explorer-keywords-history` | Historical keyword count by position range |
| `site-explorer-total-search-volume-history` | Total search volume trend for ranked keywords |
| `keywords-explorer-overview` | Metrics for specific keywords (volume, KD, CPC) |
| `keywords-explorer-matching-terms` | Find keywords matching a term |
| `keywords-explorer-related-terms` | "Also rank for" and "also talk about" keywords |
| `keywords-explorer-search-suggestions` | Google autocomplete suggestions |
| `keywords-explorer-volume-history` | Historical search volume for a keyword |
| `keywords-explorer-volume-by-country` | Volume breakdown by country |

### Backlinks
| Tool | Purpose |
|---|---|
| `site-explorer-backlinks-stats` | Backlink overview (total, dofollow, referring domains) |
| `site-explorer-all-backlinks` | Detailed backlink list with filtering |
| `site-explorer-referring-domains` | List of referring domains |
| `site-explorer-refdomains-history` | Referring domains trend over time |
| `site-explorer-broken-backlinks` | Broken backlinks (reclaim opportunities) |
| `site-explorer-anchors` | Anchor text distribution |

### Pages
| Tool | Purpose |
|---|---|
| `site-explorer-top-pages` | Top pages by organic traffic |
| `site-explorer-pages-by-backlinks` | Pages ranked by backlink count |
| `site-explorer-pages-by-traffic` | Pages by traffic bucket distribution |
| `site-explorer-pages-by-internal-links` | Internal link structure |
| `site-explorer-pages-history` | Historical page count |
| `site-explorer-paid-pages` | Pages in paid search |

### Content & Links
| Tool | Purpose |
|---|---|
| `site-explorer-linked-anchors-external` | External anchor text used by a domain |
| `site-explorer-linked-anchors-internal` | Internal anchor text |
| `site-explorer-linked-domains` | External domains linked from a site |
| `site-explorer-outlinks-stats` | Outbound link statistics |

### Technical SEO
| Tool | Purpose |
|---|---|
| `site-audit-projects` | Site Audit project summaries and health scores |
| `site-audit-issues` | All crawl issues (errors, warnings, notices) |
| `site-audit-page-explorer` | Detailed page-level audit data |
| `site-audit-page-content` | HTML/text content of crawled pages |

### SERP Analysis
| Tool | Purpose |
|---|---|
| `serp-overview` | Top search results for a keyword with full metrics |

---

## Competitive Intelligence

| Tool | Purpose |
|---|---|
| `site-explorer-organic-competitors` | Organic search competitors by keyword overlap |
| `batch-analysis` | Compare multiple domains in one call |
| `brand-radar-sov-overview` | Share of voice across AI platforms |
| `brand-radar-sov-history` | SOV trend over time |
| `brand-radar-mentions-overview` | Brand mention count in AI responses |
| `brand-radar-mentions-history` | Brand mention trend |
| `brand-radar-impressions-overview` | Estimated impressions in AI responses |
| `brand-radar-impressions-history` | Impressions trend |

---

## Brand Monitoring (AI Visibility)

| Tool | Purpose |
|---|---|
| `brand-radar-ai-responses` | Actual AI responses mentioning the brand |
| `brand-radar-cited-domains` | Domains cited alongside brand mentions |
| `brand-radar-cited-pages` | Specific pages cited in AI responses |
| `brand-radar-sov-overview` | Share of voice vs competitors |
| `brand-radar-sov-history` | SOV trend |
| `brand-radar-mentions-overview` | Total mention counts |
| `brand-radar-mentions-history` | Mention trend |
| `brand-radar-impressions-overview` | Estimated impressions |
| `brand-radar-impressions-history` | Impressions trend |
| `management-brand-radar-reports` | List configured Brand Radar reports |
| `management-brand-radar-prompts` | Get prompts for a Brand Radar report |

---

## Web Analytics

| Tool | Purpose |
|---|---|
| `web-analytics-stats` | Aggregate stats (visitors, bounce rate, duration) |
| `web-analytics-chart` | Time-series traffic data |
| `web-analytics-top-pages` | Most visited pages |
| `web-analytics-top-pages-chart` | Top pages over time |
| `web-analytics-entry-pages` | Landing pages |
| `web-analytics-exit-pages` | Exit pages |
| `web-analytics-countries` | Visitors by country |
| `web-analytics-cities` | Visitors by city |
| `web-analytics-devices` | Desktop / mobile / tablet split |
| `web-analytics-browsers` | Browser usage |
| `web-analytics-operating-systems` | OS usage |
| `web-analytics-languages` | Browser language |
| `web-analytics-source-channels` | Traffic by channel (organic, paid, social, direct) |
| `web-analytics-sources` | Traffic by source |
| `web-analytics-referrers` | Referrer URLs |
| `web-analytics-utm-params` | UTM parameter analysis |

---

## Google Search Console

| Tool | Purpose |
|---|---|
| `gsc-keywords` | GSC keywords with clicks, impressions, CTR, position |
| `gsc-keyword-history` | Keyword performance over time |
| `gsc-pages` | GSC pages with metrics |
| `gsc-page-history` | Page performance over time |
| `gsc-pages-history` | Total indexed pages over time |
| `gsc-performance-history` | Overall performance trend |
| `gsc-performance-by-device` | Performance by device type |
| `gsc-performance-by-position` | Metrics by position range |
| `gsc-positions-history` | Keyword counts by position range over time |
| `gsc-metrics-by-country` | Clicks by country |
| `gsc-ctr-by-position` | CTR by average position |
| `gsc-anonymous-queries` | Keywords not reported by GSC |

---

## Rank Tracker

| Tool | Purpose |
|---|---|
| `rank-tracker-overview` | Tracked keyword rankings and metrics |
| `rank-tracker-competitors-overview` | Competitor ranking comparison |
| `rank-tracker-competitors-pages` | Competitor pages in SERPs |
| `rank-tracker-competitors-stats` | Competitor SOV, traffic, position stats |
| `rank-tracker-serp-overview` | SERP details for a tracked keyword |

---

## Content Research & Creation

| Tool | Purpose |
|---|---|
| `keywords-explorer-matching-terms` | Find content topic ideas |
| `keywords-explorer-related-terms` | "Also talk about" topics |
| `keywords-explorer-search-suggestions` | Autocomplete ideas |
| `Apify: rag-web-browser` | Scrape and extract content from web pages |
| `Apify: search-actors` | Find specialized scrapers |
| `WebSearch` | General web search |
| `WebFetch` | Fetch and analyze a specific URL |
| `nano-banana: generate_image` | Generate marketing images |

---

## Project Management

| Tool | Purpose |
|---|---|
| `management-projects` | List Ahrefs projects |
| `management-project-keywords` | Get tracked keywords for a project |
| `management-project-competitors` | Get project competitors |
| `management-locations` | Available location targeting |
| `subscription-info-limits-and-usage` | Check API quota and usage |
