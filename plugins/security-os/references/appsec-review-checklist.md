# AppSec Manual Review Checklist

The manual playbook behind the `appsec-reviewer` agent — used standalone or to triage/extend scanner output. For each area: locate the relevant code (Grep/Glob), read it in context, and only report findings you have verified in the actual code, with file:line.

## A. OWASP Top 10 walkthrough

| # | Category | What to look for | Typical greps / entry points |
|---|---|---|---|
| A01 | Broken access control | Routes/handlers missing authz checks; IDOR (object fetched by user-supplied ID without ownership check); role checks client-side only; mass assignment | route definitions, `params.id`, ORM `findById`, middleware chains |
| A02 | Cryptographic failures | Plain/weak hashes for passwords (MD5/SHA1/unsalted); homemade crypto; HTTP for sensitive calls; secrets in localStorage; missing TLS verification (`rejectUnauthorized: false`, `verify=False`) | `md5`, `sha1`, `createCipher`, `verify=False` |
| A03 | Injection | String-built SQL; shell exec with user input; NoSQL operator injection; template injection; `eval`-family | string concat near `query(`, `exec(`, `eval(`, f-strings in SQL |
| A04 | Insecure design | Missing rate limits on auth/expensive endpoints; unbounded uploads; trust-by-obscurity (unguessable URL as auth); missing tenant isolation | auth endpoints, upload handlers, multi-tenant queries |
| A05 | Security misconfiguration | Debug mode in prod; permissive CORS (`*` with credentials); missing security headers (CSP, HSTS, X-Frame-Options); verbose error pages; default credentials | config files, CORS setup, error handlers |
| A06 | Vulnerable components | Outdated deps with known CVEs; abandoned packages; lockfile drift | `npm audit`/`pip-audit`/`osv-scanner` output; manifest review |
| A07 | Auth failures | See section B | — |
| A08 | Integrity failures | Unsigned webhooks accepted; insecure deserialization (`pickle.loads`, `yaml.load`, Java native); auto-update without verification; CI pulling unpinned actions/images | webhook handlers, `pickle`, `yaml.load`, workflow files |
| A09 | Logging & monitoring gaps | Auth events not logged; secrets/PII written to logs; no alerting on anomalies | logger calls near auth and payment code |
| A10 | SSRF | User-supplied URLs fetched server-side without allowlist; redirects followed blindly; cloud metadata endpoint reachable | `fetch(`/`requests.get(` with variable URLs, image/URL-preview features |

## B. Authentication & Authorization deep-dive

- **Passwords:** bcrypt/scrypt/argon2 with sane cost; no max-length truncation; timing-safe comparison; no user-enumeration via error/timing differences.
- **Sessions/tokens:** httpOnly+Secure+SameSite cookies; JWT — algorithm pinned (no `alg:none`, no HS/RS confusion), expiry enforced, revocation story; refresh-token rotation; logout actually invalidates.
- **Reset & recovery flows:** single-use, expiring, unguessable tokens; no account takeover via email change without re-auth.
- **Authorization:** every mutating endpoint re-checks ownership server-side; admin surfaces gated by role, not by hiding the link; multi-tenant queries always scoped by tenant ID from the session, never from the request body.
- **OAuth (if present):** `state` validated; redirect URIs exact-matched; tokens stored server-side.

## C. Secrets-in-code scan

1. Grep patterns: `api[_-]?key`, `secret`, `password\s*=`, `token`, `BEGIN (RSA|EC|OPENSSH) PRIVATE KEY`, `AKIA[0-9A-Z]{16}` (AWS), `sk_live_`, `ghp_`, `xox[bp]-`, long base64/hex literals in config.
2. Check `.gitignore` covers `.env*`; check git history (`git log -p -- .env` or a secrets scanner) — history exposure counts even if HEAD is clean.
3. Client-bundle leakage: framework public-env conventions (`NEXT_PUBLIC_`, `VITE_`) — confirm nothing sensitive is prefixed into the bundle.
4. Any confirmed exposure → recommend immediate rotation (and the incident agent if it was public).

## D. Input validation & output encoding

- Enumerate trust boundaries: request params/body/headers, file uploads, webhooks, queue messages, third-party API responses, CSV/import features.
- Validate server-side (allowlist > blocklist); client-side validation is UX, not security.
- Uploads: type sniffed not just extension, size-capped, stored outside webroot / served with safe content-type, filenames never used raw in paths.
- Output: context-appropriate encoding (HTML templating auto-escape on; `dangerouslySetInnerHTML`/`v-html`/`| safe` audited individually); JSON responses with correct content-type; no user input reflected into headers/redirects unvalidated.

## Severity guide

| Severity | Bar |
|---|---|
| CRITICAL | Remotely exploitable now, crown-jewel impact (auth bypass, SQLi on prod data, leaked live secret) |
| HIGH | Exploitable with modest effort or preconditions; serious data/account impact |
| MEDIUM | Real weakness needing specific circumstances; defense-in-depth gaps on sensitive paths |
| LOW | Hardening opportunities, best-practice deviations with limited impact |
| INFO | Observations, hygiene, things to watch |

Report only verified findings; label anything uncertain as "needs verification" rather than inflating severity.
