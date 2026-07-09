# Data Breach Notification Timeline

## GDPR Article 33-34 Procedure

### The 72-Hour Clock

**When does the clock start?**
The 72-hour period begins when the controller becomes "aware" of the breach. Awareness means the controller has a reasonable degree of certainty that a security incident has occurred that has compromised personal data.

**Key moments:**
- Employee discovers unauthorized access → awareness begins when reported to management/DPO
- Processor notifies controller of breach → awareness begins when controller receives notification
- Detection by automated systems → awareness begins when alert is reviewed and confirmed
- Discovery during audit → awareness begins when breach is confirmed

**The processor must notify the controller "without undue delay" after becoming aware (Art. 33(2)).**

---

### Timeline

| Hour | Action | Details |
|---|---|---|
| **0** | **Breach becomes known** | Clock starts. Document the exact time of awareness. |
| **0-2** | **Initial containment** | Isolate affected systems, preserve evidence, prevent further data loss. |
| **0-4** | **Incident Response Manager triage** | Classify severity, identify data types, estimate scope. |
| **0-8** | **DPO consulted** | Assess risk to individuals. Begin notification decision. |
| **0-12** | **Notification decision** | Determine if notification to supervisory authority is required. |
| **0-24** | **Draft notification** | Prepare the Art. 33 notification (can be in phases). |
| **24-48** | **Internal review** | Management review and approval of notification. |
| **48-72** | **Submit to supervisory authority** | File the notification via the DPA's portal. |
| **72** | **DEADLINE** | Notification must be filed. If not possible, document reasons for delay. |
| **Post-72h** | **Supplementary information** | Provide remaining details in phases as they become available (Art. 33(4)). |
| **Without undue delay** | **Data subject notification** | If high risk to individuals (Art. 34), notify affected persons. |

---

### Supervisory Authority Notification (Art. 33)

**Required unless** the breach is "unlikely to result in a risk to the rights and freedoms of natural persons."

#### Content of notification:

1. **Nature of the breach:**
   - Description of what happened
   - Categories of data subjects affected (e.g., customers, employees)
   - Approximate number of data subjects
   - Categories of personal data records
   - Approximate number of records

2. **DPO/contact point:**
   - Name and contact details of the DPO
   - Or other contact point for more information

3. **Likely consequences:**
   - What harm could result (identity theft, financial loss, reputational damage, discrimination)
   - Severity assessment

4. **Measures taken:**
   - Actions to address the breach
   - Actions to mitigate adverse effects
   - Remediation steps planned or underway

#### Phased notification:
If all information is not available within 72 hours, provide information in phases "without further undue delay" (Art. 33(4)):
- Phase 1 (within 72h): everything you know so far
- Phase 2+: supplementary information as the investigation progresses

---

### Data Subject Notification (Art. 34)

**Required when** the breach is "likely to result in a **high risk** to the rights and freedoms of natural persons."

**Not required if (Art. 34(3)):**
- (a) Appropriate technical protection was applied (encryption) making data unintelligible
- (b) Subsequent measures ensure high risk is no longer likely to materialize
- (c) It would involve disproportionate effort — use public communication instead

#### Content of notification to data subjects:

Must use "clear and plain language" and include:
1. Nature of the breach (what happened, in plain language)
2. DPO/contact point name and contact details
3. Likely consequences (what it means for them)
4. Measures taken and recommended actions they should take

**Communication channels:**
- Direct communication preferred (email, letter, phone)
- If disproportionate effort: public communication (website banner, press release)
- **Do not use the compromised communication channel**

---

### Decision Matrix: Do We Need to Notify?

#### Supervisory Authority (Art. 33)

| Factor | Low Risk (no notification) | Risk Present (notify) |
|---|---|---|
| Data type | Non-sensitive, already public | Special categories, financial, ID numbers |
| Volume | Single record | Multiple records / many individuals |
| Encryption | Data was encrypted, key not compromised | Unencrypted or key also compromised |
| Containment | Immediate containment, no data exfiltration | Data accessed, copied, or exfiltrated |
| Attacker | Internal, accidental, immediately corrected | External, malicious, or prolonged exposure |

**If in doubt: notify.** Under-reporting is a bigger regulatory risk than over-reporting.

#### Data Subjects (Art. 34)

Apply the **higher threshold** — "high risk" to rights and freedoms:
- Financial data that could enable fraud
- Health data that could cause discrimination
- Login credentials that could enable account takeover
- Data of vulnerable individuals (children, patients)
- Large-scale breach affecting many individuals

---

### Documentation Requirements

Regardless of whether notification is required, **document everything** (Art. 33(5)):

| Document | Content |
|---|---|
| **Breach register entry** | Facts, effects, remedial actions taken |
| **Risk assessment** | How risk was evaluated, factors considered |
| **Notification decision** | Why notification was/wasn't required, with reasoning |
| **Notification copy** | Copy of notification sent to authority/data subjects |
| **Timeline log** | Chronological record of all actions with timestamps |
| **Lessons learned** | Root cause, process improvements, preventive measures |

---

### Country-Specific Requirements

Some EU Member States have additional requirements:

| Country | Additional Requirement |
|---|---|
| **Germany** | State DPAs may have specific notification forms; sector-specific rules for telecoms (TKG) |
| **France** | CNIL online notification portal; additional ANSSI notification for NIS2 entities |
| **Netherlands** | AP has specific notification form; mandatory data breach register |
| **UK** | ICO notification within 72h; separate requirements for PECR breaches |
| **Italy** | Garante may require additional information; specific telecoms rules |

**NIS2 additional requirements (if applicable):**
- Early warning to CSIRT within 24 hours
- Incident notification to CSIRT within 72 hours
- Final report within 1 month

**DORA additional requirements (if financial sector):**
- Classify ICT incidents per DORA criteria
- Report major incidents to competent authority
