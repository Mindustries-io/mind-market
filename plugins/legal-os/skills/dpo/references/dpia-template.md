# DPIA Template — Data Protection Impact Assessment

## GDPR Article 35 Procedure

### Part 1: Screening — Is a DPIA Required?

**Mandatory DPIA triggers (Art. 35(3)):**
- [ ] Systematic and extensive profiling with significant effects on individuals
- [ ] Large-scale processing of special categories (Art. 9) or criminal data (Art. 10)
- [ ] Systematic monitoring of publicly accessible areas on a large scale

**EDPB high-risk criteria (2+ triggers a mandatory DPIA):**
- [ ] Evaluation or scoring (profiling, predicting)
- [ ] Automated decision-making with legal or significant effect
- [ ] Systematic monitoring
- [ ] Sensitive or highly personal data
- [ ] Data processed on a large scale
- [ ] Matching or combining datasets
- [ ] Data concerning vulnerable data subjects (children, employees, patients)
- [ ] Innovative use of technology
- [ ] Transfer outside the EEA
- [ ] Processing that prevents data subjects from exercising a right or using a service

**Check national DPA's mandatory DPIA list** for the relevant jurisdiction.

**Result:** DPIA required? YES / NO
**Rationale:** {document the reasoning}

---

### Part 2: Description of the Processing

| Field | Details |
|---|---|
| **Processing activity name** | |
| **Controller** | {org name, contact details} |
| **DPO** | {name, contact} |
| **Purpose(s) of processing** | |
| **Lawful basis** | Art. 6(1)({letter}): {description} |
| **Categories of data subjects** | |
| **Categories of personal data** | |
| **Special categories (Art. 9)?** | YES / NO — if yes, Art. 9(2) exception: |
| **Recipients / categories** | |
| **International transfers?** | YES / NO — if yes, transfer mechanism: |
| **Retention period** | |
| **Technical description** | {systems, data flows, storage, access} |

---

### Part 3: Necessity and Proportionality Assessment

| Principle | Assessment | Compliant? |
|---|---|---|
| **Purpose limitation** | Is processing limited to stated purposes? | YES / PARTIAL / NO |
| **Data minimization** | Is only necessary data collected? | YES / PARTIAL / NO |
| **Storage limitation** | Are retention periods defined and justified? | YES / PARTIAL / NO |
| **Accuracy** | Are measures in place to ensure data accuracy? | YES / PARTIAL / NO |
| **Lawful basis** | Is the lawful basis appropriate and documented? | YES / PARTIAL / NO |
| **Data subject rights** | Can all applicable rights be exercised? | YES / PARTIAL / NO |
| **Processor compliance** | If processors involved, are DPAs in place? | YES / PARTIAL / NO / N/A |

---

### Part 4: Risk Assessment

For each identified risk, assess:
- **Likelihood**: Remote (1) / Possible (2) / Likely (3) / Almost certain (4)
- **Severity**: Negligible (1) / Limited (2) / Significant (3) / Maximum (4)
- **Risk level**: Likelihood x Severity → LOW (1-4) / MEDIUM (5-8) / HIGH (9-12) / CRITICAL (13-16)

| # | Risk Description | Likelihood | Severity | Risk Level |
|---|---|---|---|---|
| 1 | Unauthorized access to personal data | | | |
| 2 | Accidental data loss or destruction | | | |
| 3 | Excessive data collection beyond purpose | | | |
| 4 | Inaccurate data leading to wrong decisions | | | |
| 5 | Re-identification of pseudonymized data | | | |
| 6 | Unauthorized disclosure to third parties | | | |
| 7 | Processing beyond original purpose | | | |
| 8 | Inability to exercise data subject rights | | | |
| 9 | {Add processing-specific risks} | | | |

---

### Part 5: Mitigation Measures

For each HIGH or CRITICAL risk, define mitigation measures:

| Risk # | Measure | Type | Responsible | Deadline | Residual Risk |
|---|---|---|---|---|---|
| | | Technical / Organizational | | | LOW / MEDIUM / HIGH |

**Common technical measures:**
- Encryption at rest and in transit
- Pseudonymization
- Access controls (RBAC)
- Audit logging
- Automated data retention and deletion
- Backup and recovery procedures
- Intrusion detection systems

**Common organizational measures:**
- Staff training and awareness
- Data protection policies and procedures
- Incident response plan
- Regular audits and reviews
- Vendor management and DPA requirements
- Data classification scheme
- Privacy by design reviews for changes

---

### Part 6: DPO Opinion

| Field | Details |
|---|---|
| **DPO consulted** | YES / NO |
| **DPO opinion** | {favorable / favorable with conditions / unfavorable} |
| **Conditions** | {if applicable} |
| **Date** | |

---

### Part 7: Prior Consultation (Art. 36)

Is prior consultation with the supervisory authority required?

Required when: residual risk remains HIGH after mitigation and the controller cannot sufficiently reduce it.

| Field | Details |
|---|---|
| **Prior consultation required** | YES / NO |
| **Rationale** | |
| **Authority contacted** | |
| **Submission date** | |
| **Response** | |

---

### Part 8: Approval and Review

| Field | Details |
|---|---|
| **DPIA approved by** | {name, role} |
| **Approval date** | |
| **Next review date** | {recommend annual or upon significant change} |
| **Review triggers** | {change in scope, new data categories, new recipients, security incident, regulatory change} |

---

### Document Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | | | Initial DPIA |
