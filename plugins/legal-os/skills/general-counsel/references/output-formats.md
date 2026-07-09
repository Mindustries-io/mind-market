# Legal OS Output Formats

Standard response templates for Legal OS agents. Use the appropriate format based on the type of output.

---

## 1. Compliance Status Report

```markdown
# Compliance Status Report — {ORG_NAME}
**Date:** {date} | **Frameworks:** {frameworks}

## Overall Score: {score}/100 ({grade})

### Framework Status
| Framework | Completion | Risk Level | Overdue Tasks |
|---|---|---|---|
| {framework} | {pct}% | {LOW/MEDIUM/HIGH} | {count} |

### Top Priorities
1. **{priority}** — {description} — Due: {date}
2. ...

### Evidence Gaps
- {obligation}: Missing {evidence_type}
- ...

### Recommendations
- {recommendation}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## 2. Risk Assessment

```markdown
# Risk Assessment — {topic}
**Date:** {date} | **Organization:** {ORG_NAME}

## Risk Rating: {LOW / MEDIUM / HIGH / CRITICAL}

### Summary
{1-2 sentence summary of the risk}

### Risk Factors
| Factor | Rating | Details |
|---|---|---|
| Likelihood | {1-5} | {explanation} |
| Impact | {1-5} | {explanation} |
| Detectability | {1-5} | {explanation} |
| Regulatory exposure | {1-5} | {explanation} |

### Analysis
{Detailed analysis — 3-5 paragraphs}

### Mitigations
| # | Action | Priority | Owner | Deadline |
|---|---|---|---|---|
| 1 | {action} | {HIGH/MED/LOW} | {owner} | {date} |

### Escalation
{Should this be escalated? To whom? By when?}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## 3. Contract Review Summary

```markdown
# Contract Review — {contract_title}
**Date:** {date} | **Counterparty:** {counterparty}

## Overall Risk: {GREEN / YELLOW / RED}

### Key Terms
| Term | Value | Assessment |
|---|---|---|
| Governing law | {law} | {OK / FLAGGED} |
| Term/duration | {period} | {OK / FLAGGED} |
| Liability cap | {cap} | {OK / FLAGGED} |
| Termination | {terms} | {OK / FLAGGED} |
| Data protection | {clause} | {OK / FLAGGED} |

### Flagged Clauses
1. **Section {n} — {title}**: {issue description} — **Risk: {level}**
   - **Recommendation:** {what to change}
2. ...

### Missing Clauses
- {required clause not found}
- ...

### Recommendation
{Accept / Accept with amendments / Reject / Escalate}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## 4. Incident Report

```markdown
# Incident Report — {incident_id}
**Reported:** {datetime} | **Severity:** {S1-S4} | **Status:** {ACTIVE / CONTAINED / RESOLVED}

## Timeline
| Time | Event |
|---|---|
| {datetime} | {description} |

## Classification
- **Type:** {data breach / security incident / unauthorized access / ...}
- **Personal data involved:** {YES / NO / UNKNOWN}
- **Categories of data:** {list}
- **Number of records:** {count or estimate}
- **Number of affected individuals:** {count or estimate}

## Notification Requirements
| Requirement | Deadline | Status |
|---|---|---|
| Supervisory authority (Art. 33) | {72h from awareness} | {PENDING / DONE / N/A} |
| Data subjects (Art. 34) | {without undue delay} | {PENDING / DONE / N/A} |
| Other authorities (NIS2/DORA) | {if applicable} | {PENDING / DONE / N/A} |

## Immediate Actions
1. {action} — Owner: {who} — Status: {done/pending}

## Root Cause
{analysis if known}

## Remediation Plan
1. {step}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## 5. Legal Health Check

```markdown
# Legal Health Check — {ORG_NAME}
**Date:** {date} | **Period:** {period}

## Executive Summary
{3-5 bullet points — biggest findings}

## Overall Health Grade: {A-F}

## Domain Scores
| Domain | Score | Trend | Key Issue |
|---|---|---|---|
| Regulatory compliance | {score} | {up/down/stable} | {issue} |
| Data protection | {score} | {up/down/stable} | {issue} |
| Contracts | {score} | {up/down/stable} | {issue} |
| Vendor risk | {score} | {up/down/stable} | {issue} |
| Incident readiness | {score} | {up/down/stable} | {issue} |
| Regulatory environment | {score} | {up/down/stable} | {issue} |

## Top 5 Priorities
| # | Priority | Risk | Domain | Action Required |
|---|---|---|---|---|
| 1 | {priority} | {CRITICAL/HIGH/MED} | {domain} | {action} |

## Regulatory Landscape
{Key upcoming changes and their impact}

## Recommendations
{Numbered list of strategic recommendations}

## Next Review
{Recommended date and scope}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## 6. Vendor Risk Assessment

```markdown
# Vendor Risk Assessment — {vendor_name}
**Date:** {date} | **Assessment type:** {initial / periodic / triggered}

## Risk Rating: {LOW / MEDIUM / HIGH / CRITICAL}

### Vendor Profile
| Field | Value |
|---|---|
| Company | {name} |
| Country | {country} |
| Services | {description} |
| Data processed | {categories} |
| Data volume | {volume} |

### Assessment Results
| Area | Rating | Notes |
|---|---|---|
| Data protection maturity | {1-5} | {notes} |
| Security certifications | {1-5} | {ISO 27001, SOC 2, etc.} |
| DPA adequacy (Art. 28) | {1-5} | {notes} |
| Sub-processor management | {1-5} | {notes} |
| Breach notification | {1-5} | {notes} |
| International transfers | {1-5} | {notes} |

### DPA Status
- **DPA in place:** {YES / NO}
- **Last reviewed:** {date}
- **Expires:** {date}
- **Article 28 compliant:** {YES / NO / PARTIAL}

### Recommendation
{Approve / Approve with conditions / Reject / Re-assess}

### Conditions (if applicable)
1. {condition}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## 7. Regulatory Briefing

```markdown
# Regulatory Briefing — {topic or "Weekly Scan"}
**Date:** {date} | **Jurisdictions:** {countries}

## Key Developments
### {development_title}
- **Source:** {authority / publication}
- **Date:** {date}
- **Impact:** {LOW / MEDIUM / HIGH}
- **Summary:** {2-3 sentences}
- **Action required:** {description or "Monitor only"}

## Upcoming Deadlines
| Date | Regulation | Requirement | Status |
|---|---|---|---|
| {date} | {regulation} | {requirement} | {ON TRACK / AT RISK / OVERDUE} |

## Enforcement Trends
{Brief analysis of recent enforcement patterns}

## Recommendations
1. {recommendation}

---
*AI-assisted guidance. Consult qualified legal counsel for binding decisions.*
```

## General Rules

1. **Always include the one-line disclaimer footer** shown in the templates; the full safety statement lives in `${CLAUDE_PLUGIN_ROOT}/SAFETY.md` — do not restate it per output
2. **Use risk colors consistently:** GREEN = LOW, YELLOW = MEDIUM, RED = HIGH, BLACK = CRITICAL
3. **Bold the most important findings** — readers scan before reading
4. **Include specific regulation references** (e.g., "GDPR Article 33(1)") — builds credibility
5. **Date everything** — legal context is time-sensitive
6. **Include "Next steps"** — every output should end with actionable items
