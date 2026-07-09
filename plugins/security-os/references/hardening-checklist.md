# Solopreneur Hardening Checklist

The full checklist behind the `hardening-auditor` agent. Mark each item PASS / PARTIAL / FAIL / N/A / UNKNOWN. Scoring: PASS = 1, PARTIAL = 0.5, FAIL/UNKNOWN = 0, N/A excluded. Grades: A ≥90%, B ≥75%, C ≥60%, D ≥40%, F <40%.

## 1. Identity & Access

| # | Item | What good looks like |
|---|---|---|
| 1.1 | Passkey or hardware-key MFA on primary email | Email is the master key to everything — strongest factor available, SMS not sufficient |
| 1.2 | MFA on domain registrar | Domain hijack = email + site + customers gone |
| 1.3 | MFA on cloud console(s) and code hosting | Root/owner accounts especially |
| 1.4 | MFA on payment processor and banking | Money accounts |
| 1.5 | Password manager, unique password per account | No reuse anywhere; long random master passphrase |
| 1.6 | Recovery paths reviewed | Recovery email/phone current; backup codes stored offline; no orphaned recovery via defunct addresses |
| 1.7 | OAuth grants audited (email, GitHub, cloud) | Quarterly: revoke unused third-party app access |
| 1.8 | No shared/generic admin accounts | Even solo: separate admin vs daily-use identities where the platform allows |

## 2. Cloud & SaaS Configuration

| # | Item | What good looks like |
|---|---|---|
| 2.1 | Least-privilege API keys & tokens | Scoped keys per purpose; no god-mode key in an app; expiry set where supported |
| 2.2 | No public storage buckets/objects unless intentional | Verified, not assumed |
| 2.3 | No admin dashboards, databases, or dev services exposed to the internet | DB ports closed; admin panels behind auth (+ IP allowlist/VPN if possible) |
| 2.4 | Audit/activity logging enabled | Cloud trail, GitHub audit log, login alerts on |
| 2.5 | Production/dev separation | Separate projects/accounts or at least separate credentials |
| 2.6 | Billing alerts set | Cost spike is often the first compromise signal a solo founder sees |
| 2.7 | Root/owner account locked away | Cloud root used only for break-glass; daily work on scoped identity |

## 3. Backups & Recovery

| # | Item | What good looks like |
|---|---|---|
| 3.1 | Every crown jewel backed up | Explicit mapping from crown-jewel list to backup mechanism |
| 3.2 | 3-2-1-ish coverage | ≥2 copies, ≥1 off the primary provider (provider account loss must not equal data loss) |
| 3.3 | One copy offline or immutable/versioned | Survives ransomware and a compromised cloud account |
| 3.4 | Restore actually tested | At least once, end-to-end, in the last 12 months — an untested backup is a hope |
| 3.5 | Backup credentials separate | The key that writes backups cannot delete them |

## 4. Domain, DNS & Email

| # | Item | What good looks like |
|---|---|---|
| 4.1 | Registrar lock / transfer lock enabled | Plus auto-renew and current payment method |
| 4.2 | SPF record | Single valid record, ends in `~all` or `-all`, under 10 DNS lookups |
| 4.3 | DKIM | Signing enabled for every legitimate sender (mail provider, transactional email) |
| 4.4 | DMARC | Published, at least `p=quarantine` (target `p=reject`), `rua=` reporting on |
| 4.5 | DNSSEC | Enabled where the registrar/TLD supports it |
| 4.6 | Stale DNS records pruned | No CNAMEs to deprovisioned services (subdomain takeover) |

## 5. Endpoint Basics

| # | Item | What good looks like |
|---|---|---|
| 5.1 | Full-disk encryption | FileVault / BitLocker / LUKS on every work device |
| 5.2 | OS + browser auto-updates on | And actually restarting to apply them |
| 5.3 | Screen lock with short timeout | Device password/biometric required |
| 5.4 | Separate browser profile for admin/financial accounts | Limits token theft via extensions |
| 5.5 | Extensions minimal and audited | Every extension can read every page |
| 5.6 | Device findable/wipeable | Find My / equivalent enabled |

## 6. Secrets Management

| # | Item | What good looks like |
|---|---|---|
| 6.1 | No secrets in source code or git history | Scan (e.g. gitleaks/trufflehog or manual grep) — history counts, not just HEAD |
| 6.2 | Secrets in env/secret manager, not files synced to cloud drives or chat | `.env` in `.gitignore`; production secrets in the platform's secret store |
| 6.3 | Rotation on suspicion or departure | Any possibly-exposed secret rotated immediately; know how to rotate each one |
| 6.4 | CI secrets scoped | Repo/environment-scoped, not org-wide; no secrets in build logs |
| 6.5 | Signing/deploy keys inventoried | You can list every key that can push code or deploy |

## Top-5 selection heuristic

Rank failed items by (attack-class neutralized × crown-jewel exposure) ÷ effort. Perennial best-value fixes: passkey on email (1.1), registrar lock (4.1), tested restore (3.4), closing an exposed dashboard/DB (2.3), rotating a leaked secret (6.3).
