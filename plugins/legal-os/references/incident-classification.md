# Incident Classification Framework

## Severity Levels

### S1 — Critical

**Definition:** Large-scale breach involving sensitive personal data with confirmed data exfiltration or ongoing unauthorized access.

**Indicators:**
- Special category data (Art. 9) confirmed compromised
- Financial data (bank accounts, credit cards) exposed
- Identity documents or government IDs compromised
- >10,000 records or >1,000 individuals affected
- Ongoing exfiltration not yet contained
- Ransomware with data theft (double extortion)
- Breach of encrypted data where keys also compromised

**Response timeline:**
- Immediate containment actions
- DPO and management notified within 1 hour
- Authority notification almost certainly required
- Data subject notification likely required
- External forensics engagement recommended

**Notification:** Authority YES (Art. 33) | Data subjects YES (Art. 34)

---

### S2 — High

**Definition:** Confirmed breach of personal data, contained, with moderate risk to individuals.

**Indicators:**
- Personal data confirmed accessed or exfiltrated
- 100-10,000 records or 10-1,000 individuals affected
- Contact details, employment data, or behavioral data exposed
- Breach contained but data already accessed
- Unauthorized internal access to personal data
- Lost or stolen device with unencrypted personal data
- Misdirected email with personal data to wrong recipients

**Response timeline:**
- Containment within hours
- DPO notified within 4 hours
- Authority notification likely required (case-by-case assessment)
- Data subject notification may be required

**Notification:** Authority LIKELY (Art. 33) | Data subjects CASE-BY-CASE (Art. 34)

---

### S3 — Medium

**Definition:** Potential breach under investigation, or confirmed incident with low risk to individuals.

**Indicators:**
- Suspicious activity detected but breach not confirmed
- <100 records potentially affected
- Non-sensitive data (name, business email) exposed
- Encrypted data breached but keys are secure
- Brief unauthorized access, quickly terminated
- Phishing attempt with uncertain data compromise
- Misconfiguration exposing data briefly before correction

**Response timeline:**
- Investigation within 24 hours
- DPO informed for awareness
- Authority notification unlikely but document the decision
- Data subject notification unlikely

**Notification:** Authority UNLIKELY (document rationale) | Data subjects NO

---

### S4 — Low

**Definition:** Near-miss or minor incident with no personal data compromise.

**Indicators:**
- Failed attack attempt (blocked by security controls)
- Vulnerability discovered but not exploited
- Lost device with full disk encryption, strong password, remote wipe activated
- Misdirected email containing no personal data
- Internal policy violation with no data exposure
- Successful phishing but credentials changed before misuse

**Response timeline:**
- Log for records
- Investigate root cause within the week
- No notification required

**Notification:** Authority NO | Data subjects NO

---

## Classification Decision Tree

```
Is personal data involved?
├── NO → S4 (Low) — log and monitor
└── YES
    ├── Was the data actually accessed/exfiltrated?
    │   ├── NO (but could have been) → S3 (Medium) — investigate
    │   └── YES
    │       ├── Is it special category data (Art. 9)?
    │       │   ├── YES → S1 (Critical)
    │       │   └── NO
    │       │       ├── >1,000 individuals affected?
    │       │       │   ├── YES → S1 (Critical)
    │       │       │   └── NO
    │       │       │       ├── >10 individuals affected?
    │       │       │       │   ├── YES → S2 (High)
    │       │       │       │   └── NO → S3 (Medium)
    │       │       ├── Financial or ID data?
    │       │       │   └── YES → Upgrade to S1 or S2
    │       │       └── Is the breach ongoing?
    │       │           └── YES → Upgrade by one level
    └── UNKNOWN (under investigation)
        └── S3 (Medium) — treat as potential until confirmed
```

## Escalation Matrix

| Severity | Notify DPO | Notify Management | Notify Authority | Notify Subjects | External Counsel | Forensics |
|---|---|---|---|---|---|---|
| **S1** | Immediate | Immediate | Within 72h | Likely | Recommended | Recommended |
| **S2** | Within 4h | Within 8h | Likely within 72h | Case-by-case | If complex | If needed |
| **S3** | Within 24h | If escalates | Unlikely | No | No | No |
| **S4** | FYI | No | No | No | No | No |

## GDPR Notification Decision Factors

Consider these factors when deciding whether to notify the supervisory authority (Art. 33):

**Factors favoring notification:**
- Any special category data involved
- Financial data that could enable fraud
- Data sufficient for identity theft
- Large number of individuals affected
- Vulnerable data subjects (children, patients, employees)
- Data was unencrypted
- Breach was result of deliberate attack
- Data confirmed accessed or exfiltrated

**Factors against notification (risk unlikely):**
- Data was strongly encrypted, keys not compromised
- Breach quickly contained with no data access
- Data was already publicly available
- Very limited scope (single record, non-sensitive)
- Immediate and effective remediation

**When in doubt: NOTIFY.** The regulatory risk of under-reporting is greater than over-reporting.
