# ObligoBoard API Integration Guide

## Overview

ObligoBoard is the compliance management platform that Legal OS agents use for obligation tracking, risk scoring, and evidence management. This guide documents the integration approach.

## Phase 1: Snapshot-Based Integration (Current)

### Snapshot File
`~/.claude/plugins/data/legal-os/compliance-snapshot.json`

### Expected Structure

```json
{
  "updated_at": "2026-04-05T10:00:00Z",
  "organization": {
    "id": "org_...",
    "name": "Acme GmbH",
    "plan": "starter|pro"
  },
  "compliance_score": {
    "overall": 72,
    "grade": "C",
    "breakdown": {
      "obligation_completion": 68,
      "profile_modifiers": 80
    }
  },
  "frameworks": [
    {
      "id": "fw_...",
      "name": "GDPR Basics",
      "slug": "gdpr-basics",
      "completion_pct": 75,
      "total_obligations": 8,
      "completed_obligations": 6,
      "obligations": [
        {
          "id": "obl_...",
          "title": "Records of Processing Activities (RoPA)",
          "description": "Maintain records of all processing activities...",
          "risk_level": "high",
          "tasks": [
            {
              "id": "task_...",
              "title": "Create and maintain a register of processing activities",
              "status": "completed|in_progress|not_started|overdue",
              "due_date": "2026-03-31",
              "assigned_to": "John Doe",
              "evidence": [
                {
                  "id": "ev_...",
                  "filename": "ropa_register.xlsx",
                  "uploaded_at": "2026-03-15"
                }
              ]
            }
          ]
        }
      ]
    }
  ],
  "risk_scores": {
    "current": 72,
    "previous": 65,
    "trend": "improving",
    "risk_level": "MEDIUM",
    "factors": [
      {
        "name": "DPIA not completed",
        "impact": -8,
        "category": "obligation_completion"
      }
    ]
  },
  "overdue_summary": {
    "total": 3,
    "by_framework": {
      "gdpr-basics": 2,
      "data-processor": 1
    },
    "by_risk_level": {
      "high": 1,
      "medium": 2,
      "low": 0
    }
  }
}
```

### How to Refresh the Snapshot

Instruct the user to export their ObligoBoard data:
1. Log in to ObligoBoard
2. Navigate to Dashboard
3. Use the export feature (when available)
4. Save the JSON to `~/.claude/plugins/data/legal-os/compliance-snapshot.json`

Alternatively, a scheduled task can automate this via browser automation (future enhancement).

## Phase 2: Direct API Integration (Future)

When ObligoBoard implements token-based API authentication:

### Authentication
```
Authorization: Bearer {API_KEY}
```

API key stored in config at `profiles.{slug}.obligoboard.api_key`.

### Key Endpoints

| Endpoint | Method | Purpose |
|---|---|---|
| `/api/tasks` | GET | List all obligation tasks with status |
| `/api/tasks/{id}` | GET | Individual task details |
| `/api/tasks/{id}/evidence` | GET | Evidence attachments for a task |
| `/api/dashboard/stats` | GET | Dashboard statistics and scores |
| `/api/frameworks` | GET | Active compliance frameworks |
| `/api/frameworks/{id}` | GET | Framework details with obligations |
| `/api/risk-score` | GET | Current risk score with factors |
| `/api/assessment` | GET | Assessment results |
| `/api/reports/evidence-pack` | GET | Generate evidence pack report |
| `/api/tools/policy-details` | GET | Organization policy configuration |

### Response Formats

All endpoints return JSON. Tasks include:
- `status`: "not_started", "in_progress", "completed", "overdue"
- `due_date`: ISO date string
- `risk_level`: "low", "medium", "high"
- `evidence`: array of evidence attachments

## Database Schema Reference

Key tables in ObligoBoard's PostgreSQL database:

### Core Tables
- `frameworks` — compliance frameworks (GDPR, Data Processor, etc.)
- `obligations` — individual obligations within frameworks
- `tasks` — organization-specific tasks for obligations
- `task_evidence` — evidence files attached to tasks
- `organization_frameworks` — org-framework associations

### Risk & Assessment
- `risk_scores` — current risk score per org
- `risk_score_audit` — historical risk score changes
- `assessments` — compliance assessment questionnaires
- `assessment_responses` — assessment answers

### Policy & Documents
- `org_policy_details` — DPO, retention, processor info
- `generated_documents` — privacy/cookie policy generation history
- `document_versions` — version tracking for generated documents

### Advanced
- `conditional_obligations` — profile-triggered obligations
- `org_cookie_inventory` — cookie categorization
- `cookie_scans` — website cookie scanning
