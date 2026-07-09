# Legal OS Tool Registry

Complete catalog of tools available to Legal OS agents, organized by function.

## Core Tools (All Agents)

### Memory & Context
| Tool | Purpose |
|---|---|
| `claude-mem: search` | Search cross-session memory for legal context (use `los:` prefix) |
| `claude-mem: get_observations` | Fetch full details of specific memory entries |
| `claude-mem: timeline` | Get context around memory entries |

### File Operations
| Tool | Purpose |
|---|---|
| `Read` | Read config files, data files, user-provided documents |
| `Write` | Write/update data files (config, registries, logs) |
| `Glob` | Find files by pattern |
| `Grep` | Search file contents |

### Research
| Tool | Purpose |
|---|---|
| `WebSearch` | Search the web for regulatory news, legal guidance, enforcement actions |
| `WebFetch` | Fetch and analyze specific web pages |
| `Apify: rag-web-browser` | Scrape web content for legal research |

---

## Built-in Legal Skills (via Skill tool)

These Anthropic marketplace skills serve as composable primitives. Invoke via `Skill("legal:skill-name", args)`.

| Skill | Primary Agent | Purpose |
|---|---|---|
| `legal:review-contract` | Contract Manager | Clause-by-clause contract analysis with redlines |
| `legal:triage-nda` | Contract Manager | GREEN/YELLOW/RED NDA classification |
| `legal:signature-request` | Contract Manager | Pre-signature checklist and routing |
| `legal:compliance-check` | Compliance Officer | Regulatory compliance assessment for actions/features |
| `legal:legal-risk-assessment` | General Counsel | Standalone risk scoring with severity classification |
| `legal:vendor-check` | Vendor Risk | Vendor agreement status across systems |
| `legal:meeting-briefing` | General Counsel | Meeting prep with legal context |
| `legal:brief` | General Counsel | Legal briefings (daily, topic, incident) |
| `legal:legal-response` | Legal Researcher | Templated responses (DSAR, litigation hold, vendor) |

---

## Agent-Specific Tools

### Compliance Officer
| Tool | Purpose |
|---|---|
| Read `compliance-snapshot.json` | ObligoBoard cached compliance data |
| Read `regulatory-calendar.json` | Upcoming compliance deadlines |
| `legal:compliance-check` | Assess compliance of proposed actions |

### DPO
| Tool | Purpose |
|---|---|
| Read `compliance-snapshot.json` | GDPR-filtered obligation data |
| `WebSearch` | EDPB guidance, DPA decisions, regulatory updates |
| `Apify: rag-web-browser` | Scrape EDPB, national DPA, and EUR-Lex content |

### Contract Manager
| Tool | Purpose |
|---|---|
| Read user-provided documents | Contract files for review |
| Read/Write `contracts/index.json` | Contract repository tracking |
| `legal:review-contract` | Detailed contract analysis |
| `legal:triage-nda` | NDA risk classification |
| `legal:signature-request` | Signature preparation |

### Legal Researcher
| Tool | Purpose |
|---|---|
| `WebSearch` | Legal research, case law, regulatory guidance |
| `Apify: rag-web-browser` | Extract content from legal databases, EUR-Lex |
| `legal:legal-response` | Generate templated legal responses |

### Regulatory Intelligence
| Tool | Purpose |
|---|---|
| `WebSearch` | Scan for regulatory news, enforcement actions |
| `Apify: rag-web-browser` | Scrape EDPB, national DPAs, EUR-Lex, EIOPA |
| Read/Write `regulatory-calendar.json` | Update compliance deadlines |

### Incident Response Manager
| Tool | Purpose |
|---|---|
| Read/Write `incident-log.json` | Incident tracking and documentation |
| `mcp__scheduled-tasks` | Create deadline-tracking tasks (72-hour notification) |
| `WebSearch` | DPA notification portal lookup |

### Vendor Risk Assessor
| Tool | Purpose |
|---|---|
| Read/Write `vendor-registry.json` | Vendor tracking and compliance status |
| `WebSearch` | Vendor reputation research, security posture |
| `legal:vendor-check` | Vendor agreement status check |

---

## Data File Locations

All data files are at `~/.claude/plugins/data/legal-os/`:

| File | Read/Write | Used By |
|---|---|---|
| `config.json` | Read (all), Write (setup) | All agents |
| `compliance-snapshot.json` | Read | Compliance Officer, DPO |
| `contracts/index.json` | Read/Write | Contract Manager |
| `regulatory-calendar.json` | Read/Write | Regulatory Intel, Compliance Officer |
| `incident-log.json` | Read/Write | Incident Response |
| `vendor-registry.json` | Read/Write | Vendor Risk |
| `matters/{id}.json` | Read/Write | General Counsel, any specialist |

---

## Important Notes

1. **Always read config.json first** — every agent needs org context before acting
2. **Use `los:` prefix for all claude-mem entries** — keeps legal memory distinct from other systems
3. **Check snapshot freshness** — if `compliance-snapshot.json` is older than 7 days, warn the user
4. **Include disclaimer** — all outputs should note "AI-assisted guidance, not legal advice"
5. **Respect escalation threshold** — if risk exceeds the org's configured threshold, flag for human review
