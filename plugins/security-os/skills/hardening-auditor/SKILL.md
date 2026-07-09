---
name: hardening-auditor
description: "Security posture and hardening specialist for a solopreneur stack. Use when the user asks to harden their setup, wants a security posture check or security checklist, asks about MFA/passkeys coverage, cloud or SaaS configuration review, public buckets, exposed dashboards, backups and restore testing, domain/DNS/email security (SPF, DKIM, DMARC), endpoint basics, or secrets management. Produces a scored checklist and top-5 actions."
---

Delegate this request to the `security-os:hardening-auditor` subagent via the Agent tool
(subagent_type: "security-os:hardening-auditor"). Pass the user's request plus org context
from the config. Do not perform the work inline; relay the agent's result.
If the Agent tool is unavailable, read `${CLAUDE_PLUGIN_ROOT}/agents/hardening-auditor.md`
and follow its workflow inline instead.
