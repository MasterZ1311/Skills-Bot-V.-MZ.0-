# 🔒 Workflow 04 — Secure & Audit a Codebase

This workflow takes an existing codebase from untested to security-hardened and compliance-ready. Run this before any major release, after a security incident, or when preparing for a pentest.

---

## 🎯 Goal
Find, fix, and prevent security vulnerabilities across your entire application stack.

**Example:** Hardening the college events platform before launch

---

## ⏱️ Timeline

| Phase | Time |
|---|---|
| Threat Modeling | 2-4 hours |
| SAST Setup | Half a day |
| Manual Pentesting | 1-2 days |
| Cloud Security Audit | Half a day |
| Compliance Review | Half a day |
| Hardening & Fixes | 1-3 days |
| **Total** | **~3-5 days** |

---

## Phase 1 — Threat Modeling

### Step 1.1 — System Threat Model
```
@threat-modeling-expert Perform threat modeling for our events platform using STRIDE:

System overview:
- Web app: Next.js (Vercel)
- API: Node.js/Express (Vercel serverless)
- Database: PostgreSQL (Neon)
- Auth: JWT + Google OAuth
- Payments: Stripe
- File storage: Supabase Storage
- Users: students, organizers, admins

Trust boundaries:
- Internet → Vercel Edge (TLS termination)
- Edge → Serverless Functions
- Functions → Database (private network)
- Functions → Stripe API (external)

For each STRIDE category, list:
- Specific threat relevant to our system
- Affected component
- Current mitigation (if any)
- Recommended mitigation

Priority: Critical and High threats get immediate action.
```

**Output:** STRIDE threat matrix, prioritized threat list

### Step 1.2 — Attack Surface Mapping
```
@attack-tree-construction Build attack trees for our top 3 threats:

Attack Tree 1: Get a free ticket without paying
Root: Obtain valid ticket for paid event without payment
├── Exploit registration API
│   ├── Skip payment step by manipulating API directly
│   ├── Race condition: register before payment confirmed
│   └── Replay attack: reuse captured request
├── Steal ticket from another user
│   ├── IDOR: access other users' tickets by guessing ID
│   └── Account takeover (steal credentials)
└── Forge ticket QR
    ├── Predict ticket ID algorithm (sequential IDs?)
    └── Find QR validation bypass

Attack Tree 2: Access admin dashboard without authorization
Attack Tree 3: Extract all student email addresses from database

For each leaf: likelihood (Low/Medium/High) + impact + test method.
```

---

## Phase 2 — Automated Scanning (SAST)

### Step 2.1 — SAST Configuration
```
@sast-configuration Set up SAST for our Node.js + React codebase:

Tool 1: ESLint Security Plugin
npm install --save-dev eslint-plugin-security @typescript-eslint/eslint-plugin
Rules to enable:
- no-eval (code injection)
- detect-non-literal-regexp (ReDoS)
- detect-object-injection (prototype pollution)
- detect-possible-timing-attacks (timing oracle)

Tool 2: Semgrep
semgrep --config=p/nodejs-security --config=p/jwt --config=p/react
Rules covering: SQL injection, XSS, SSRF, weak crypto, path traversal

Tool 3: npm audit + Snyk
npm audit --audit-level=high
snyk test --severity-threshold=high

GitHub Actions: add to PR pipeline
- Block merge if: CRITICAL Semgrep finding OR npm audit CRITICAL
- Report: comment on PR with finding summary
```

### Step 2.2 — Custom Semgrep Rules
```
@semgrep-rule-creator Write custom Semgrep rules for our codebase:

Rule 1: Detect direct SQL string concatenation
Pattern:
  query = "SELECT * FROM events WHERE id = " + userId
  // Also catches template literals: `SELECT * FROM ... ${id}`
Action: ERROR — use parameterized queries

Rule 2: Detect missing auth on API routes
Pattern: any Next.js route handler that doesn't call getServerSession() or verifyJWT()
Action: WARNING — check if this is intentionally public

Rule 3: Detect console.log of request/response
Pattern: console.log(req.body) or console.log(user) in any route handler
Action: WARNING — may log sensitive data

Rule 4: Detect MD5 or SHA1 for password hashing
Pattern: crypto.createHash('md5') or createHash('sha1')
Action: ERROR — use argon2 or bcrypt

Test each rule: provide true positive + true negative example.
```

### Step 2.3 — Dependency Audit
```
@codebase-cleanup-deps-audit Audit and clean dependencies:

Step 1: npm audit
- List all HIGH and CRITICAL vulnerabilities
- For each: is the package actually used? Can we update?
- Update all with available patches

Step 2: License audit
- List packages with GPL/AGPL licenses (may be incompatible with commercial SaaS)
- Recommend alternatives for any incompatible ones

Step 3: Outdated packages
- List all packages > 2 major versions behind
- Prioritize: packages with frequent CVEs (express, jsonwebtoken, axios)

Step 4: Remove unused packages
- Find packages in package.json not imported anywhere
- Safe to remove: reduces attack surface

Output: npm update commands for all safe updates.
```

---

## Phase 3 — Manual Penetration Testing

### Step 3.1 — Authentication Testing
```
@broken-authentication Test our authentication system:
JWT implementation audit:
1. Algorithm confusion: does our verifier accept 'none' algorithm?
   Test: modify JWT header to alg: "none", remove signature → does it work?
2. Secret strength: is our JWT_SECRET at least 256 bits of entropy?
3. Expiry enforcement: does API reject tokens past their 'exp' claim?
4. Refresh token rotation: is old refresh token invalidated after use?
5. Logout: does API reject tokens after logout? (requires blocklist)
6. Rate limiting: can we brute force login? (try 100 attempts)

Google OAuth:
1. State parameter: protected against CSRF?
2. Nonce: replay attack prevention?
3. Email verification: do we trust unverified Google emails?

Session:
1. httpOnly: is refresh token cookie httpOnly + Secure + SameSite=Strict?
2. Cookie theft via XSS: does any XSS exist that could steal cookies?
```

### Step 3.2 — IDOR Testing
```
@idor-testing Test for Insecure Direct Object References:
All endpoints that take an ID parameter:

Test 1: Ticket access (/api/tickets/:ticketId)
- Create account A, register for event → get ticket_id_A
- Create account B
- As account B: GET /api/tickets/ticket_id_A
- Expected: 403 Forbidden (not 200 or 404 with data)

Test 2: Registration access (/api/registrations/:registrationId)
- As account B: GET /api/registrations/registration_id_A
- Expected: 403

Test 3: Event management (/api/events/:eventId)
- Create organizer A's event → event_id_A
- Create organizer B account
- As organizer B: PATCH /api/events/event_id_A (try to update A's event)
- Expected: 403

Test 4: Admin endpoints (/api/admin/*)
- As regular student: GET /api/admin/users
- Expected: 403

If any returns 200 with data: CRITICAL vulnerability.
```

### Step 3.3 — Injection Testing
```
@sql-injection-testing + @xss-html-injection Test for injection vulnerabilities:

SQL Injection:
Target endpoints (all that accept user input):
- GET /api/events?search=PAYLOAD
- GET /api/events?category=PAYLOAD
- POST /api/auth/login (email field)

Payloads to test:
- ' OR '1'='1' -- (classic)
- ' AND SLEEP(5) -- (time-based blind)
- '; DROP TABLE events; -- (destructive, test in dev ONLY)

Since we use Prisma ORM: all queries should be parameterized.
Verify: inspect generated SQL with prisma.$queryRawUnsafe (should not exist in our code)

XSS:
Target: any field that gets rendered back to users
- Event title: <script>alert(1)</script>
- Event description: same
- Username: same
- URL params rendered on page: /events?search=<script>

React protects against XSS by default — verify dangerouslySetInnerHTML is NEVER used.
Check: rich text editor for event descriptions (WYSIWYG can introduce XSS).
```

### Step 3.4 — API Fuzzing
```
@api-fuzzing-bug-bounty Fuzz our registration endpoint:
POST /api/events/:eventId/register

Input fuzzing — try each:
- eventId: null, "", 0, -1, "admin", "' OR 1=1", 999999999999, "a".repeat(1000)
- ticketCount: 0, -1, 1.5 (float), 999999, null, "one"
- paymentMethodId: null, "", "pm_invalid", "pm_" + "a".repeat(500)
- Extra unexpected fields: __proto__, constructor, isAdmin: true

Race condition test:
- 2 threads simultaneously POST registration for the last ticket
- Expected: only 1 succeeds, 1 gets 409 (sold out)
- Use: Apache JMeter or custom Python threading script

Large payload:
- Description field: 1MB of text
- Expected: 413 Payload Too Large or validation error (not crash)
```

---

## Phase 4 — Cloud Security Audit

### Step 4.1 — AWS / Vercel Security
```
@security/aws-security-audit Audit our cloud security (Vercel + AWS RDS):

Vercel:
- Environment variables: are secrets in Vercel env vars (not hardcoded)?
- Preview deployments: do previews have access to production secrets?
  (They should use separate preview-env secrets)
- Headers: are security headers set? (CSP, HSTS, X-Frame-Options)

AWS (if using RDS, S3, etc.):
- RDS: is it in a private subnet (no public endpoint)?
- S3: are buckets private by default? Any accidental public buckets?
- IAM: does our Lambda/ECS role have least-privilege?
- Security Groups: port 5432 open to internet? (Should be restricted to app only)
- CloudTrail: enabled?

Supabase (if using):
- RLS: is it enabled on all tables?
- Service role key: never exposed in client-side code?
- Storage: are private buckets actually private?
```

### Step 4.2 — Security Headers Audit
```
@backend-security-coder Add security headers to our Next.js app:
In next.config.ts, add headers():

Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'unsafe-inline' https://js.stripe.com;
  connect-src 'self' https://api.stripe.com https://vitals.vercel-insights.com;
  img-src 'self' data: https://*.supabase.co;
  frame-src https://js.stripe.com;

Other headers:
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
  Strict-Transport-Security: max-age=31536000; includeSubDomains

Test with: securityheaders.com after deployment.
Goal: A+ rating.
```

---

## Phase 5 — Compliance Review

### Step 5.1 — GDPR / Indian Privacy Law
```
@gdpr-data-handling Review our data handling for compliance:
Data we collect: email, name, college, event registration history, payment amounts

Data mapping:
- What we collect → why we need it → how long we keep it
- Email: required for account, registration confirmation, reminders → keep while account active
- Payment amounts: financial record keeping → 7 years (tax law)
- Event attendance: feature functionality → keep 2 years, then anonymize

Required documents:
- Privacy Policy: what we collect, why, how long, user rights
- Cookie Policy: only essential cookies? Or analytics?
- Data deletion: implement DELETE /api/me endpoint (right to erasure)
- Data export: implement GET /api/me/export (right to portability)
- Consent: registration checkbox with explicit consent text

For India's DPDPA (Digital Personal Data Protection Act):
- Data localization: student data must be on India servers
- Data fiduciary: we are the fiduciary, organizers are processors
- Grievance officer: designate one, publish contact
```

### Step 5.2 — Payment Security (PCI DSS)
```
@pci-compliance Review our Stripe integration for PCI compliance:
Our implementation: Stripe.js (client) → Stripe API (server)
This means: we NEVER handle raw card data ourselves

Our PCI scope:
- We qualify for SAQ A (the simplest, lowest burden self-assessment)
- Requirements:
  1. Don't store card data (we don't — Stripe does)
  2. Use Stripe.js for card input (in-scope form not on our server)
  3. Maintain secure connection to Stripe (TLS 1.2+)
  4. Keep Stripe library up to date
  5. Restrict access to Stripe dashboard to authorized people only

Document: SAQ A completion and annual review process.
```

---

## Phase 6 — Fix & Harden

### Step 6.1 — Container & Deployment Hardening
```
@container-security-hardening Harden our Docker containers:
Current: running as root user, all ports exposed, no health check

Fixes:
1. Run as non-root: add USER node in Dockerfile (port > 1024)
2. Read-only filesystem: --read-only flag (exceptions: /tmp, log dirs)
3. Drop capabilities: --cap-drop ALL --cap-add NET_BIND_SERVICE (if needed)
4. No new privileges: --security-opt=no-new-privileges
5. Resource limits: --memory 512m --cpus 0.5 (prevent DoS)
6. Health check: HEALTHCHECK CMD curl -f http://localhost:3000/health || exit 1
7. Minimal base image: node:20-alpine (not node:20) — fewer attack surfaces
8. No dev dependencies in prod: npm ci --omit=dev

Scan: trivy image our-events-api:latest → fix all CRITICAL CVEs
```

### Step 6.2 — Rate Limiting
```
@api-security-best-practices Implement rate limiting on all API endpoints:
Library: @upstash/ratelimit (Redis-based, works on Vercel Edge)

Limits by endpoint:
- POST /api/auth/login: 5 requests/minute per IP (brute force protection)
- POST /api/auth/register: 3 requests/hour per IP (spam prevention)
- POST /api/registrations: 10 requests/minute per user (normal usage)
- GET /api/events: 60 requests/minute per IP (generous, public data)
- POST /api/tickets/validate: 30 requests/minute per organizer

Headers to return:
- X-RateLimit-Limit: max requests
- X-RateLimit-Remaining: requests left in window
- X-RateLimit-Reset: Unix timestamp when limit resets
- Retry-After (on 429): seconds until retry is safe

Response on limit exceeded: 429 Too Many Requests + JSON error body.
```

---

## ✅ Security Audit Completion Checklist

```
Threat Modeling:
□ STRIDE analysis completed for all components
□ Attack trees built for top 3 threats
□ All Critical threats have documented mitigations

Automated Scanning:
□ ESLint security plugin: 0 warnings in CI
□ Semgrep: 0 Critical or High findings
□ npm audit: 0 Critical or High CVEs
□ Custom Semgrep rules: deployed and catching issues

Manual Pentesting:
□ Auth: JWT algorithm confusion tested → not vulnerable
□ Auth: refresh token rotation verified
□ IDOR: all object references properly authorized
□ SQLi: all endpoints tested → parameterized queries confirmed
□ XSS: no dangerouslySetInnerHTML, no unescaped output
□ Race condition: concurrent registration tested → atomic
□ Fuzzing: malformed inputs handled gracefully (no 500 errors)

Cloud Security:
□ Security headers: A+ on securityheaders.com
□ S3/Storage: no public buckets except intended CDN assets
□ Database: not publicly accessible
□ Secrets: no secrets in code or environment files committed to git
□ Container: non-root user, no critical CVEs

Compliance:
□ Privacy Policy: published and linked from footer
□ Cookie consent: implemented (if using analytics cookies)
□ Data deletion: /api/me DELETE endpoint working
□ PCI SAQ A: completed and signed

Documentation:
□ Security incident response plan written
□ Responsible disclosure policy published
□ Security findings logged in issue tracker with severity
□ All Critical and High findings resolved
□ Medium findings have assigned owner and deadline
```
