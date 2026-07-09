# Incident Response Playbook (solopreneur edition)

Operating document for the `security-incident` agent. Containment-first: it is almost always better to briefly break your own service than to let an attacker keep access.

## Severity

| Level | Definition | Tempo |
|---|---|---|
| SEV1 | Active attacker, crown jewels exposed, or production compromised | Drop everything; actions in minutes |
| SEV2 | Confirmed compromise with contained blast radius (one key, one account) | Same day, methodical |
| SEV3 | Suspicious signal, unconfirmed | Investigate within 24h; don't panic-rotate everything yet |

## Phase 1 — Triage (first 15 minutes)

1. **Log everything from minute zero.** Plain text file, one timestamped line per observation/action. This is the source of truth for insurance, notification deadlines, and the post-mortem.
2. Establish: what was observed, when first seen, what asset is affected, is it ongoing?
3. Assign severity. When in doubt between two levels, pick the higher.
4. Note the awareness time explicitly — regulatory clocks (e.g. GDPR 72h) run from awareness, not from resolution.

## Phase 2 — Contain

Pick the smallest action that cuts attacker access. Common scenarios:

| Scenario | First moves |
|---|---|
| Leaked API key / secret (e.g. pushed to a public repo) | Revoke & rotate the key immediately (rotation beats history-rewriting — treat the secret as burned even after force-push); check provider logs for use since exposure |
| Compromised account (email, cloud, GitHub) | Change password from a known-clean device, revoke all sessions/tokens, remove unknown OAuth grants/app passwords, check mail forwarding/filter rules, re-enroll MFA |
| Compromised server/container | Isolate (security group/firewall to deny-all) rather than power off — preserves memory/evidence; snapshot disk if possible |
| Malware/ransomware on endpoint | Disconnect from network; do NOT pay or wipe yet; work from a second clean device |
| Data exposure (public bucket, open DB) | Close the exposure first, then pull access logs to scope who reached it |
| Supply-chain (malicious dependency) | Pin/remove the package, rotate every secret the build/runtime could read, audit CI logs |

Rules: prefer reversible actions; preserve evidence (disconnect, don't wipe); rotate credentials **after** removing attacker access (else they watch you rotate).

## Phase 3 — Scope

- Pull every relevant log: cloud audit trail, provider access logs, GitHub audit log, auth logs, mail rules, webhook/API-key creation events.
- Build a timeline: first attacker action → detection → containment.
- Enumerate what was readable/writable during the window. Assume read = taken for anything sensitive.
- **Answer explicitly: was personal data possibly affected?** Categories, approximate subject count, sensitivity. This gates the legal handoff (agent workflow step 4).

## Phase 4 — Eradicate & Recover

1. Remove persistence: rogue users/keys/OAuth grants/mail rules/cron jobs/webhooks/deploy keys; reimage compromised machines rather than "cleaning" them.
2. Close the entry point (patch, config fix, revoked credential) — recovery without this is an invitation back.
3. Rotate every secret within the blast radius, in dependency order (root/owner first).
4. Restore data from a known-good backup taken before first attacker action; verify integrity before going live.
5. Reconnect gradually with elevated monitoring (login alerts, billing alerts, log review daily for two weeks).

## Phase 5 — Post-mortem (within a week)

Blameless. Sections: timeline · root cause (technical + process) · what worked · what failed · 3-5 follow-up actions with dates. Feed follow-ups to `hardening-auditor`; store the summary to memory as `sec:incident:<date-slug>`.

## Notification quick reference (fallback when legal-os is absent)

- **GDPR Art. 33:** personal-data breach → notify supervisory authority within **72 hours of awareness** unless unlikely to risk individuals; document the breach either way.
- **GDPR Art. 34:** high risk to individuals → notify affected people without undue delay.
- Contracts (enterprise customers, DPAs) and cyber-insurance policies often have their **own, shorter** notice clocks — check them during Phase 3, not after recovery.
- This is a reminder, not legal advice — engage counsel or `legal-os:incident-response` for the actual determination.
