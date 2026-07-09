# Vendor Risk Assessment Criteria

## Assessment Framework

Each vendor is evaluated across 8 domains. Score each domain 1-5 (1 = critical gaps, 5 = excellent). The overall risk rating is derived from the weighted average.

---

## Domain 1: Data Protection Maturity (Weight: 20%)

| Score | Criteria |
|---|---|
| **5** | Designated DPO, documented privacy program, regular audits, privacy by design embedded |
| **4** | Privacy policies in place, staff trained, some monitoring |
| **3** | Basic privacy measures, policies exist but not regularly updated |
| **2** | Minimal privacy measures, awareness but no formal program |
| **1** | No privacy program, no DPO, no documented measures |

**Evidence to request:**
- Privacy policy (internal and external)
- Records of processing activities
- DPO appointment document
- Privacy training records
- DPIA examples

---

## Domain 2: Security Posture (Weight: 20%)

| Score | Criteria |
|---|---|
| **5** | ISO 27001 + SOC 2 Type II current, regular pen testing, bug bounty, SIEM |
| **4** | One major certification current, regular pen testing, security monitoring |
| **3** | Security measures documented, some testing, no current certification |
| **2** | Basic security measures, no testing, no certification |
| **1** | No documented security measures, no certifications, no testing |

**Evidence to request:**
- ISO 27001 certificate (check validity)
- SOC 2 Type II report (request bridge letter if >6 months old)
- Most recent penetration test summary
- Security architecture overview
- Incident response plan

---

## Domain 3: DPA Compliance (Weight: 20%)

| Score | Criteria |
|---|---|
| **5** | All 13 Art. 28 elements present, strong positions, ≤24h breach notification |
| **4** | All mandatory elements present, reasonable positions, ≤48h breach notification |
| **3** | Most elements present, some gaps, ≤72h breach notification |
| **2** | Significant elements missing, weak positions |
| **1** | No DPA or fundamentally inadequate |

**Use the DPA checklist** in `dpa-checklist.md` for detailed evaluation.

---

## Domain 4: Sub-Processor Management (Weight: 10%)

| Score | Criteria |
|---|---|
| **5** | Prior specific written authorization, current sub-processor list, back-to-back DPAs, notification of changes |
| **4** | Prior general authorization with right to object (30+ days), current list |
| **3** | General authorization with short objection period, list available on request |
| **2** | Sub-processor list not readily available, unclear approval mechanism |
| **1** | No sub-processor management, no visibility |

**Evidence to request:**
- Current sub-processor list (with locations and services)
- Sub-processor approval mechanism description
- Sub-processor DPA template or standard
- Change notification process

---

## Domain 5: International Transfers (Weight: 10%)

| Score | Criteria |
|---|---|
| **5** | EU/EEA processing only, or adequate country, no transfers |
| **4** | Transfers with SCCs + TIA, supplementary measures implemented |
| **3** | Transfers with SCCs, TIA completed but supplementary measures pending |
| **2** | Transfers occurring, SCCs in place but no TIA |
| **1** | Transfers without adequate safeguards |

**Evidence to request:**
- Data flow diagram (showing all processing locations)
- SCCs (if applicable)
- Transfer Impact Assessment
- Supplementary measures documentation

---

## Domain 6: Breach Notification Capability (Weight: 10%)

| Score | Criteria |
|---|---|
| **5** | Documented breach procedure, ≤24h notification to controller, tested regularly |
| **4** | Documented procedure, ≤48h notification, tested at least annually |
| **3** | Procedure exists, ≤72h notification, not regularly tested |
| **2** | Informal process, notification timeframe unclear |
| **1** | No breach notification process documented |

**Evidence to request:**
- Incident response plan (relevant sections)
- Contractual breach notification commitment
- Record of breach notification drills
- Contact details for breach reporting

---

## Domain 7: Business Continuity (Weight: 5%)

| Score | Criteria |
|---|---|
| **5** | DR/BC plans tested annually, RTO/RPO documented, multi-region redundancy |
| **4** | DR/BC plans documented, tested, reasonable recovery targets |
| **3** | Plans exist but not regularly tested |
| **2** | Informal continuity measures only |
| **1** | No business continuity planning |

---

## Domain 8: Contractual Cooperation (Weight: 5%)

| Score | Criteria |
|---|---|
| **5** | Full audit rights, responsive to requests, transparent, willing to negotiate |
| **4** | Audit via third-party reports, reasonably responsive |
| **3** | Limited audit options, slow to respond |
| **2** | Resistant to audit, unresponsive to data protection queries |
| **1** | Uncooperative, refuses reasonable requests |

---

## Overall Risk Rating Calculation

| Weighted Average | Rating | Recommendation |
|---|---|---|
| **4.0 - 5.0** | LOW | Approve |
| **3.0 - 3.9** | MEDIUM | Approve with conditions |
| **2.0 - 2.9** | HIGH | Approve only if business-critical, with enhanced monitoring |
| **1.0 - 1.9** | CRITICAL | Reject or require fundamental remediation before engagement |

## Automatic Escalation Triggers

Regardless of overall score, **automatically flag as HIGH or CRITICAL** if:
- No DPA in place (Domain 3 score ≤ 2)
- Special category data processed without adequate measures
- International transfers without safeguards (Domain 5 score ≤ 2)
- Security breach in the last 12 months with inadequate response
- Refusal to provide audit evidence or certification
