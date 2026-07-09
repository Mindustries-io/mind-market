# Breach Notification Templates

## Template 1: Supervisory Authority Notification (Art. 33)

### Initial Notification (within 72 hours)

```
DATA BREACH NOTIFICATION — Article 33 GDPR

1. CONTROLLER DETAILS
   Organization: {ORG_NAME}
   Address: {address}
   Contact person: {DPO name}
   Contact email: {DPO email}
   Contact phone: {phone}

2. NATURE OF THE BREACH
   Date and time of breach: {datetime}
   Date and time of awareness: {datetime}
   Description: {clear description of what happened}

   Type of breach:
   [ ] Confidentiality (unauthorized disclosure or access)
   [ ] Integrity (unauthorized alteration)
   [ ] Availability (unauthorized destruction or loss)

3. DATA SUBJECTS AFFECTED
   Categories: {e.g., customers, employees, subscribers}
   Approximate number of data subjects: {number or estimate}

4. PERSONAL DATA RECORDS AFFECTED
   Categories of data: {e.g., name, email, address, financial data}
   Approximate number of records: {number or estimate}
   Special categories (Art. 9): {YES/NO — if yes, specify}

5. LIKELY CONSEQUENCES
   {Description of potential impact on data subjects, e.g.:
   - Risk of identity theft/fraud
   - Financial loss
   - Reputational damage
   - Discrimination
   - Other significant effects}

6. MEASURES TAKEN
   Containment measures: {what was done to stop the breach}
   Remediation measures: {what is being done to fix the cause}
   Mitigation measures: {what is being done to reduce impact on data subjects}
   Data subject notification: {planned / not required — with rationale}

7. ADDITIONAL INFORMATION
   {Any supplementary details}
   Note: We will provide further information as our investigation progresses,
   in accordance with Article 33(4).

Date of notification: {date}
Submitted by: {name, role}
```

### Supplementary Notification (follow-up)

```
SUPPLEMENTARY BREACH NOTIFICATION — Article 33(4) GDPR

Reference: {original notification reference number}
Original notification date: {date}

This supplementary notification provides additional information
regarding the breach notified on {date}.

UPDATED INFORMATION:
{Provide updates to any of the sections from the initial notification,
clearly marking what has changed}

INVESTIGATION FINDINGS:
{Summary of investigation results since last notification}

ADDITIONAL MEASURES:
{Any new containment, remediation, or mitigation measures taken}

Date: {date}
Submitted by: {name, role}
```

---

## Template 2: Data Subject Notification (Art. 34)

### Individual Notification Letter/Email

```
Subject: Important Notice About Your Personal Data

Dear {Name / "Valued Customer/User"},

We are writing to inform you about a security incident that may
have affected your personal data.

WHAT HAPPENED
{Plain language description — 2-3 sentences explaining what occurred,
when it was discovered, and that it has been contained}

WHAT DATA WAS INVOLVED
{List the specific categories of data that may have been affected,
e.g.:
- Your name and email address
- Your account login credentials
- [Other relevant categories]}

WHAT WE ARE DOING
{Description of actions taken:
- How we contained the incident
- Steps to prevent recurrence
- We have notified {supervisory authority name} as required by law}

WHAT YOU CAN DO
We recommend you take the following precautions:
{Specific, actionable recommendations, e.g.:
- Change your password for your account and any other accounts
  where you used the same password
- Monitor your accounts for unusual activity
- Be cautious of unsolicited communications asking for personal
  information (phishing attempts)
- [Additional recommendations based on data type]}

CONTACT US
If you have questions or concerns, please contact our Data Protection
Officer:

   Name: {DPO name}
   Email: {DPO email}
   Phone: {phone}

You also have the right to lodge a complaint with {supervisory authority
name} ({authority URL}).

We sincerely apologize for this incident and are committed to
protecting your personal data.

Sincerely,
{Name}
{Title}
{Organization}
```

### Public Communication (when individual notification is disproportionate effort)

```
IMPORTANT DATA SECURITY NOTICE

{Organization} has become aware of a security incident that may have
affected the personal data of some of our {customers/users/members}.

WHAT HAPPENED: {Brief description}

WHEN: The incident occurred on {date} and was discovered on {date}.

WHO IS AFFECTED: {Description of affected group}

WHAT DATA: {Categories of data involved}

WHAT WE ARE DOING:
- {Containment action}
- {Investigation status}
- {Notification to {authority name}}
- {Remediation measures}

WHAT YOU SHOULD DO:
- {Recommendation 1}
- {Recommendation 2}

For more information, please contact:
{DPO name} at {DPO email} or {phone}

Posted: {date}
Last updated: {date}
```

---

## Template 3: Internal Incident Report

```
INTERNAL INCIDENT REPORT — CONFIDENTIAL

Incident ID: {ID}
Classification: {S1/S2/S3/S4}
Status: {ACTIVE / CONTAINED / UNDER INVESTIGATION / RESOLVED}

TIMELINE
{date time} — {event description}
{date time} — {event description}
...

SUMMARY
{2-3 paragraph description of the incident}

IMPACT ASSESSMENT
- Personal data involved: YES/NO
- Categories of data: {list}
- Number of records: {count}
- Number of individuals: {count}
- Special categories: YES/NO

CONTAINMENT ACTIONS
1. {Action taken — by whom — when}

ROOT CAUSE
{Analysis of how the incident occurred}

NOTIFICATION DECISIONS
- Supervisory authority: {YES/NO — rationale}
- Data subjects: {YES/NO — rationale}
- Other regulators: {YES/NO — rationale}

REMEDIATION
1. {Short-term fix — status}
2. {Long-term prevention — status}

LESSONS LEARNED
1. {Lesson}
2. {Process improvement}

Report prepared by: {name}
Date: {date}
Next review: {date}
```

---

## Template 4: NIS2 Incident Notification (if applicable)

```
NIS2 INCIDENT NOTIFICATION

EARLY WARNING (within 24 hours of awareness):
- Suspected significant incident: YES
- Suspected malicious/unlawful cause: {YES/NO/UNKNOWN}
- Cross-border impact potential: {YES/NO/UNKNOWN}

INCIDENT NOTIFICATION (within 72 hours):
- Updated assessment of severity and impact
- Indicators of compromise
- Technical details of the incident

FINAL REPORT (within 1 month):
- Detailed description of the incident
- Severity and impact
- Type of threat or root cause
- Applied and ongoing mitigation measures
- Cross-border impact if applicable

Submitted to: {National CSIRT / competent authority}
```
