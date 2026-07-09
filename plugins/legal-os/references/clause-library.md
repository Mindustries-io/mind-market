# Contract Clause Library

Standard and risky clause patterns for contract review. Use these as benchmarks when reviewing counterparty drafts.

---

## 1. Governing Law and Jurisdiction

### Standard (acceptable)
- Governing law of the org's country of incorporation
- Courts of the org's principal place of business

### Acceptable alternatives
- Governing law of counterparty's country (within EU) if commercial relationship warrants it
- Arbitration under ICC, DIS, LCIA, or other recognized institution
- Mediation-first clauses with escalation to arbitration/courts

### Red flags
- Exclusive jurisdiction in a non-EU country without strong commercial justification
- Governing law of a jurisdiction with weak contract enforcement
- "Any court of competent jurisdiction" (too vague)
- Mandatory arbitration with no appeal rights in high-value contracts

---

## 2. Limitation of Liability

### Standard positions
- **Liability cap:** 12 months of contract value (most common)
- **Acceptable range:** 6-24 months depending on risk
- **Carve-outs from cap:** willful misconduct, gross negligence, IP infringement, confidentiality breach, data protection breach, death/personal injury

### Red flags
- Unlimited liability for the org (one-sided)
- Liability cap below 6 months of contract value
- No carve-outs for data protection breaches
- Exclusion of indirect/consequential damages where the org is the customer (losing right to claim)
- "To the maximum extent permitted by law" exclusions (too broad)

---

## 3. Indemnification

### Standard (acceptable)
- Mutual indemnification for IP infringement claims
- Indemnification for breaches of confidentiality
- Indemnification for willful misconduct / gross negligence
- Defense obligation (not just payment)

### Red flags
- One-sided indemnification favoring counterparty only
- Broad indemnification for "any claims arising from use of the service"
- Indemnification without liability cap
- No obligation to mitigate damages
- No right to control the defense

---

## 4. Data Protection / DPA

### Required elements (GDPR Art. 28)
- Subject matter and duration of processing
- Nature and purpose of processing
- Type of personal data and categories of data subjects
- Obligations and rights of the controller
- Processing only on documented controller instructions
- Confidentiality obligations for processing personnel
- Appropriate technical and organizational security measures
- Sub-processor engagement conditions (prior authorization)
- Assistance with data subject rights
- Assistance with DPIA and prior consultation
- Deletion or return of data at end of contract
- Audit and inspection rights

### Red flags
- No DPA despite personal data processing
- DPA that doesn't include all Art. 28(3) requirements
- Sub-processor engagement without prior written authorization
- Breach notification timeframe >72 hours (should be ≤48h to give controller time)
- No audit rights or "desk audit only" limitations
- Data retention after contract termination without clear purpose
- Processing location restrictions missing

---

## 5. Confidentiality

### Standard (acceptable)
- Mutual confidentiality obligations
- Clear definition of "Confidential Information"
- Standard exceptions (public knowledge, independent development, legally compelled)
- Duration: term + 2-5 years (or indefinite for trade secrets)
- Return or destruction of confidential information at termination

### Red flags
- One-sided (only org has obligations)
- Overly broad definition catching non-sensitive information
- No exceptions for legally compelled disclosure
- Duration less than 2 years
- No return/destruction obligation
- Residual knowledge clause that's too broad

---

## 6. Termination

### Standard (acceptable)
- Termination for convenience with 30-90 day notice
- Termination for material breach with cure period (30 days typical)
- Termination for insolvency/bankruptcy
- Survival clauses (confidentiality, liability, IP, data protection)

### Red flags
- No termination for convenience (lock-in)
- One-sided termination rights
- Automatic renewal without adequate notice period
- Termination penalties that exceed reasonable costs
- No cure period for breach
- Short notice period (<30 days) for the org
- Data not returned upon termination

---

## 7. Intellectual Property

### Standard (acceptable)
- Each party retains pre-existing IP
- Work product IP ownership clearly assigned
- License grants clearly scoped
- No assignment of background IP

### Red flags
- Broad IP assignment clause covering org's pre-existing IP
- Unlimited license to org's data or content
- No IP warranties (freedom from third-party claims)
- Work-for-hire without clear scope definition
- "All improvements" belong to one party regardless of creator

---

## 8. Force Majeure

### Standard (acceptable)
- Lists specific events (natural disasters, war, government actions, pandemics)
- Obligation to mitigate
- Right to terminate if force majeure continues beyond 60-90 days
- Notification obligation within reasonable timeframe

### Red flags
- Overly broad (includes economic hardship, market changes)
- No obligation to mitigate
- No termination right after extended force majeure
- One-sided (only protects counterparty)

---

## 9. Service Level Agreement (SLA)

### Standard (acceptable)
- Defined availability targets (99.5%+ for SaaS)
- Clear measurement methodology
- Service credits for downtime (5-30% of monthly fee)
- Exclusions for scheduled maintenance
- Escalation procedures

### Red flags
- No SLA in a service contract
- Availability targets below 99%
- Service credits as sole remedy (no termination right for chronic failure)
- Exclusions so broad they gut the SLA
- No measurement transparency

---

## 10. Auto-Renewal

### Standard (acceptable)
- Clear renewal notification with adequate notice period (60-90 days before renewal)
- Easy opt-out process
- Price adjustment caps or transparency
- Same terms for renewal period

### Red flags
- Auto-renewal with <30 day notice requirement
- Price increases at renewal without cap
- No opt-out mechanism
- Terms can change at renewal without consent
- Renewal period longer than initial term
