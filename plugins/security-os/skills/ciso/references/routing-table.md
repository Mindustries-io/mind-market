# CISO Routing Table (extended)

Hub-and-spoke (Model A) is the default. Pipeline (B) passes output between agents in sequence. Collaborative (C) fans out in parallel and synthesizes — announce the agent list before running B or C.

## Model A — single specialist (FAST MODE)

| Example prompts | Agent |
|---|---|
| "Review the security of my API", "is my auth solid?", "check this PR for vulnerabilities", "any secrets in my repo?", "run npm audit and triage it", "OWASP check on the upload endpoint" | `security-os:appsec-reviewer` |
| "Threat model my new billing feature", "what could go wrong if I add file sharing?", "attack surface of my architecture", "security design review before launch" | `security-os:threat-modeler` |
| "Harden my setup", "security posture check", "do I have MFA everywhere I should?", "is my DMARC configured?", "are my backups good enough?", "lock down my cloud account", "where should my secrets live?" | `security-os:hardening-auditor` |
| "Is Vendor X safe to use?", "security due diligence on this analytics tool", "does this SaaS have SOC 2?", "has this provider been breached?", "should I give this app access to my repos?" | `security-os:vendor-security` |
| "I think I've been hacked", "my API key leaked on GitHub", "weird logins on my account", "customer data may have been exposed", "ransomware note on my laptop" | `security-os:security-incident` (EMERGENCY — route instantly) |
| "Set up security-os", "change my stack inventory", "update crown jewels" | `/security-os:setup` skill |

## Model B — pipelines

| Scenario | Sequence |
|---|---|
| Pre-launch check of a new feature | `threat-modeler` → `appsec-reviewer` (model the design, then verify the code against the top threats) |
| Post-incident hardening | `security-incident` (post-mortem) → `hardening-auditor` (fold follow-ups into the posture checklist) |
| Vendor adoption end-to-end | `vendor-security` → hand off to `legal-os:vendor-risk` if installed (DPA/contract side) |

## Model C — collaborative

| Scenario | Agents |
|---|---|
| Full security health check | `hardening-auditor` + `appsec-reviewer` + `threat-modeler` + `vendor-security`, CISO synthesis |
| "Am I ready to sign my first enterprise customer?" | `hardening-auditor` + `appsec-reviewer` (+ note: formal attestations need a professional auditor) |

## Routing tie-breakers

- Mentions of compromise, leak, or "right now" → always `security-incident` first, even if the request also fits another agent.
- "Secure" about **code the user owns** → `appsec-reviewer`; about **accounts/infra/ops** → `hardening-auditor`; about a **design not yet built** → `threat-modeler`; about **someone else's product** → `vendor-security`.
- Offensive requests (exploits, attacking third parties, malware) → decline per `${CLAUDE_PLUGIN_ROOT}/SAFETY.md`; offer the defensive equivalent.
