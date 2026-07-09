# Compliance Data Sources

How Legal OS agents get compliance status data (obligations, scores, evidence). Two modes;
both are optional — agents always work, they just say less precisely where data is missing.

## Mode 1: Compliance tracker export (snapshot)

If the organization uses a compliance-tracking tool (any tool — for example the fictional
**Aurora** by Acme Studio at `aurora.example.com`, or a spreadsheet-based tracker), the user
exports a JSON or CSV snapshot and saves it to:

```
~/.claude/plugins/data/legal-os/compliance-snapshot.json
```

### Freshness check

Check the snapshot's `updated_at` field:
- **< 7 days old** — use the data as-is.
- **7-14 days old** — use it, but warn the user it may be stale.
- **> 14 days old or missing** — warn the user to re-export, proceed with limited data.

### Expected JSON structure

Field names are a convention, not a contract — map whatever the user's tool exports onto
this shape as best you can, and ask about ambiguous fields.

```json
{
  "updated_at": "2026-04-05T10:00:00Z",
  "organization": { "name": "Acme Studio" },
  "compliance_score": {
    "overall": 72,
    "grade": "C",
    "breakdown": { "obligation_completion": 68, "profile_modifiers": 80 }
  },
  "frameworks": [
    {
      "name": "GDPR Basics",
      "completion_pct": 75,
      "obligations": [
        {
          "title": "Records of Processing Activities (RoPA)",
          "risk_level": "high",
          "tasks": [
            {
              "title": "Create and maintain a register of processing activities",
              "status": "completed | in_progress | not_started | overdue",
              "due_date": "2026-03-31",
              "assigned_to": "Owner name",
              "evidence": [ { "filename": "ropa_register.xlsx", "uploaded_at": "2026-03-15" } ]
            }
          ]
        }
      ]
    }
  ],
  "risk_scores": {
    "current": 72, "previous": 65, "trend": "improving", "risk_level": "MEDIUM",
    "factors": [ { "name": "DPIA not completed", "impact": -8 } ]
  },
  "overdue_summary": {
    "total": 3,
    "by_framework": { "gdpr-basics": 2, "data-processor": 1 },
    "by_risk_level": { "high": 1, "medium": 2, "low": 0 }
  }
}
```

### CSV exports

If the tool only exports CSV, accept a file with (at minimum) columns for framework,
obligation/task title, status, due date, and owner. Convert it mentally to the structure
above; note any fields the export lacks (e.g. evidence, risk scores).

### Refreshing the snapshot

Instruct the user: export from your compliance tool's dashboard or reporting feature and
save the file to the path above. There is no live API integration — the snapshot is the
single data interface.

## Mode 2: Lite mode (no tracker)

If there is no snapshot and the user has no compliance tool:
1. Ask the user to paste whatever they have — a task list, spreadsheet rows, or a plain
   description of what is done and what is open.
2. Structure the pasted data into the framework/obligation/task shape above for analysis.
3. Provide qualitative assessments (gaps, priorities, recommendations) instead of precise
   scores; clearly mark any score as an estimate.
4. Offer to save the structured result to `compliance-snapshot.json` so future sessions
   can reuse it.
