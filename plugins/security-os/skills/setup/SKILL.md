---
name: setup
description: "Security OS configuration and onboarding. Use when the user asks to 'set up security', 'configure security os', 'security os setup', 'sos setup', 'configure my security profile', 'update my stack inventory', 'change crown jewels', 'security settings', or mentions setting up security-os for the first time."
---

# Security OS Setup Wizard

Configure the Security OS profile stored at `~/.claude/plugins/data/security-os/config.json`. Full schema: `${CLAUDE_PLUGIN_ROOT}/skills/setup/references/config-schema.md` (read it before writing the file).

## Flow

Ask in conversational batches (2-3 questions at a time), not a 20-question form. Accept "skip" for anything — every field except `org_name` has a sensible default.

### 1. Org profile
- Org/product name, primary domain, what the product does (one line), solo or small team?

### 2. Stack inventory
- Languages/frameworks (e.g. TypeScript/Next.js, Python/Django)
- Hosting/cloud (e.g. Vercel, AWS, Hetzner) and where production data lives
- Code hosting & CI (e.g. GitHub + Actions)
- Key SaaS tools with access to code, data, or money (email provider, payment processor, analytics, AI tools)

### 3. Crown jewels
- "What data or system would hurt most if leaked, tampered with, or lost?" — capture 1-5 items (e.g. customer PII, production DB, Stripe account, source code, the founder's email account).

### 4. Existing controls
- MFA/passkeys where? Password manager? Backups (and ever restore-tested)? Secrets manager or `.env` files? Any monitoring/alerts? Capture free-form; the hardening-auditor verifies later.

### 5. Risk appetite & cross-OS
- Risk appetite: conservative / moderate / aggressive (default: moderate).
- Detect sibling plugins: check whether `legal-os` skills are available (e.g. the skill list contains `legal-os:general-counsel`); set `cross_os.legal_os_installed` accordingly, and ask rather than guess if unsure.

## Writing the config

1. Read the existing config first (if any) and merge — never clobber other profiles.
2. Create the directory if needed, write JSON matching the schema, set `created_at`/`updated_at` (ISO dates), set `active_profile`.
3. Show the user the resulting JSON and confirm.

## After setup

Suggest the natural first step: "Run a posture check (`hardening-auditor`) to get your baseline score, or ask the `ciso` for a full security health check."

Updates ("change my stack", "add a crown jewel") follow the same flow scoped to the field in question.
