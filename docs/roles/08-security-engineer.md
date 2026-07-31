# 🔒 Security Engineer — Skills Guide

Security engineers find and fix vulnerabilities, run penetration tests, enforce compliance, and harden systems. This guide covers auditing, penetration testing, SAST, threat modeling, compliance, and cloud security.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| Auditing | `@security-audit`, `@cyber-audit`, `@production-code-audit`, `@cc-skill-security-review` |
| Penetration Testing | `@ethical-hacking-methodology`, `@burp-suite-testing`, `@metasploit-framework` |
| SAST / Scanning | `@sast-configuration`, `@semgrep-rule-creator`, `@vulnerability-scanner` |
| Attack Vectors | `@sql-injection-testing`, `@xss-html-injection`, `@api-fuzzing-bug-bounty`, `@idor-testing` |
| Compliance | `@pci-compliance`, `@gdpr-data-handling`, `@privacy-by-design` |
| Cloud Security | `@security/aws-security-audit`, `@security/aws-iam-best-practices`, `@k8s-security-policies` |
| Threat Modeling | `@threat-modeling-expert`, `@attack-tree-construction`, `@stride-analysis-patterns` |
| Hardening | `@container-security-hardening`, `@api-security-best-practices`, `@secrets-management` |

---

## 🔍 Security Auditing

### `@security-audit` / `@security-auditor`
Comprehensive security audit of a codebase or service.
```
@security-audit Perform a security audit of our events platform:
- Authentication and authorization flaws
- Input validation gaps
- SQL injection risks
- XSS vulnerabilities in templates
- Insecure direct object references (IDOR)
- Sensitive data in logs/responses
- Dependency vulnerabilities
Produce a finding report with severity (Critical/High/Medium/Low) and remediation.
```

### `@cyber-audit`
Cyber security audit from an attacker's perspective.
```
@cyber-audit Red team audit our registration endpoint (POST /api/events/:id/register):
- Can I register without authentication?
- Can I bypass capacity limits?
- Can I register for someone else's email?
- Can I cause a double-charge via race condition?
- What happens if I send malformed payment data?
```

### `@production-code-audit` / `@codebase-audit-pre-push`
Automated audit before pushing to production.
```
@codebase-audit-pre-push Run pre-production security checklist:
- No hardcoded secrets (API keys, passwords)
- No console.log of sensitive data
- All user inputs sanitized
- Authentication on all protected endpoints
- Rate limiting in place
- CORS configured correctly
- Error messages don't expose stack traces
```

---

## 🔴 Penetration Testing

### `@ethical-hacking-methodology`
Structured pentest methodology: recon → scanning → exploitation → reporting.
```
@ethical-hacking-methodology Plan a penetration test for our events platform.
Scope: web application (events.college.edu) and REST API (api.events.college.edu)
Out of scope: production database direct access, DDoS

Phase 1: Information gathering
Phase 2: Vulnerability scanning
Phase 3: Manual exploitation
Phase 4: Post-exploitation
Phase 5: Report

Give me the methodology and tools for each phase.
```

### `@burp-suite-testing` / `@burpsuite-project-parser`
Burp Suite interception, scanning, active testing.
```
@burp-suite-testing Set up Burp Suite for testing our events API:
- Configure proxy for our API (localhost:3000)
- Create Burp project for college events pentest
- Set up scanner for active scanning
- Configure exclusions (avoid hitting payment endpoints in staging)
- Export findings in format for our report
```

### `@metasploit-framework`
Metasploit for exploitation testing.
```
@metasploit-framework Our server runs Apache 2.4.49. 
What CVEs are relevant? Show how to test for CVE-2021-41773 (path traversal).
What is the remediation? Generate a finding for our report.
```

### `@pentest-checklist` / `@pentest-commands`
```
@pentest-checklist Generate a web application pentest checklist for our events platform:
OWASP Top 10 coverage:
- Injection (SQL, command, LDAP)
- Broken authentication
- Sensitive data exposure
- XXE
- Broken access control (IDOR, privilege escalation)
- Security misconfiguration
- XSS
- Insecure deserialization
- Using components with known vulnerabilities
- Insufficient logging
```

---

## 🔬 SAST & Scanning

### `@sast-configuration`
Static Application Security Testing setup.
```
@sast-configuration Set up SAST for our Node.js + React codebase:
Tools to configure:
- ESLint security plugin (@eslint-community/eslint-plugin-security)
- Semgrep rules for Node.js
- npm audit for dependency vulnerabilities
- Snyk for deeper dependency analysis
Integrate all into GitHub Actions CI — fail PR on Critical/High findings.
```

### `@semgrep-rule-creator` / `@semgrep-rule-variant-creator`
Custom Semgrep rules for your codebase.
```
@semgrep-rule-creator Write custom Semgrep rules for our codebase:
1. Detect direct SQL string concatenation (SQL injection risk)
2. Detect use of MD5 for passwords (weak hashing)
3. Detect console.log of request body (data leak)
4. Detect missing authentication decorator on controller methods
Test each rule with a true positive and true negative example.
```

### `@vulnerability-scanner` / `@scanning-tools`
Automated vulnerability scanning.
```
@vulnerability-scanner Set up a vulnerability scanning pipeline:
- Container image scanning: Trivy in CI (block on Critical CVEs)
- DAST: OWASP ZAP baseline scan on staging
- Dependency scanning: Dependabot + Snyk
- Infrastructure: Checkov for Terraform
- Reporting: findings aggregated to Slack weekly
```

---

## 🎯 Attack Vector Testing

### `@sql-injection-testing`
```
@sql-injection-testing Test our events search endpoint for SQL injection:
Endpoint: GET /api/events?search=<query>
- Classic: ' OR '1'='1
- Boolean-based blind
- Time-based blind (SLEEP/pg_sleep)
- Error-based extraction
Our ORM: Prisma (should parameterize). Verify it does.
What manual tests should we run? What tools (SQLMap)?
```

### `@xss-html-injection` / `@html-injection-testing`
```
@xss-html-injection Test our events platform for XSS:
Inputs to test: event title, description, organizer name, search query
XSS types: reflected, stored, DOM-based
Payloads to try and what to observe.
How does our React frontend protect us? What can bypass React's escaping?
```

### `@api-fuzzing-bug-bounty`
```
@api-fuzzing-bug-bounty Fuzz our registration API endpoint:
POST /api/events/:id/register
- Invalid eventId formats (null, 0, -1, SQL, very long strings)
- Negative ticketCount, decimal count, maxInt
- Missing required fields
- Extra unexpected fields (mass assignment?)
- Concurrent requests (race condition for last ticket)
What fuzzing tool? (ffuf, wfuzz, custom script)
```

### `@idor-testing`
```
@idor-testing Test our tickets endpoint for IDOR:
GET /api/tickets/:ticketId
- Can user A access user B's ticket by guessing/incrementing ID?
- Sequential vs UUID ID comparison
- Test with two test accounts
- Check if authorization is on ticket fetch or just on list
```

### `@file-path-traversal`
```
@file-path-traversal Test our file upload endpoint for path traversal:
POST /api/events/:id/image (accepts image upload)
- Filename: ../../../etc/passwd
- Filename: ..%2F..%2F..%2Fetc%2Fpasswd
- Double encoding
- Null byte injection
What should the server validate? Show secure implementation.
```

### `@broken-authentication`
```
@broken-authentication Test our authentication system:
- JWT algorithm confusion (RS256 vs HS256)
- JWT secret brute force feasibility
- Token not invalidated on logout
- Password reset token reuse
- Session fixation
- Rate limiting on login endpoint
```

---

## 📋 Compliance

### `@pci-compliance`
Payment Card Industry compliance for apps handling card data.
```
@pci-compliance Review our payment integration for PCI DSS compliance:
We use Stripe.js (never touch raw card data).
Our API: creates payment intents, stores customer ID.
What is our PCI scope? What SAQ level applies?
What do we need to document and implement for SAQ A compliance?
```

### `@gdpr-data-handling`
GDPR data handling for EU users.
```
@gdpr-data-handling Implement GDPR compliance for our events platform:
Data we collect: email, name, phone, registration history, payment amounts
- Data minimization: do we collect more than needed?
- Right to deletion: implement user data delete endpoint
- Data portability: export user data as JSON
- Consent management: registration checkbox with explicit consent
- Data retention: auto-delete registrations after 2 years
```

### `@privacy-by-design`
```
@privacy-by-design Review our events platform for privacy by design:
- Minimize data collection to what's strictly necessary
- Pseudonymize where possible
- Access control: only admin sees personal data
- Audit logging: who accessed what data when
- Data breach response plan
```

---

## ☁️ Cloud Security

### `@security/aws-security-audit`
```
@security/aws-security-audit Audit our AWS environment:
- IAM: least privilege violations, unused roles
- S3: public buckets, no encryption
- Security groups: 0.0.0.0/0 rules on ports other than 80/443
- CloudTrail: gaps in logging
- GuardDuty: enable and check findings
- Config: compliance rules setup
```

### `@security/aws-iam-best-practices`
```
@security/aws-iam-best-practices Harden our IAM setup:
- Enable MFA for all human users
- Remove root access keys
- Create separate roles per service (not one shared role)
- Use permission boundaries for developer roles
- Enable Access Analyzer
- Rotate access keys older than 90 days
```

### `@k8s-security-policies`
```
@k8s-security-policies Harden our Kubernetes cluster:
- Pod Security Standards: Baseline → Restricted for app namespaces
- Network Policies: deny-all default, explicit allow only
- RBAC: no cluster-admin for app service accounts
- Secret encryption at rest (etcd)
- Image policy: only pull from our private registry
- Admission webhooks: OPA/Gatekeeper policies
```

---

## 🧩 Threat Modeling

### `@threat-modeling-expert`
Structured threat identification for a system.
```
@threat-modeling-expert Perform threat modeling for our event registration system.
Use STRIDE methodology.
Assets: user data, payment data, event data, ticket QR codes
Entry points: web app, REST API, admin dashboard, mobile app
Trust boundaries: public internet → API → DB

For each STRIDE category, list threats and mitigations.
```

### `@attack-tree-construction`
Build attack trees for specific threats.
```
@attack-tree-construction Build an attack tree for:
Goal: Attacker gains free access to a sold-out event

Root: Obtain valid ticket without paying
├── Forge QR code
│   ├── Steal valid QR from another user
│   ├── Predict QR code algorithm
│   └── Find QR validation bypass
├── Register without payment
│   ├── Race condition on last spot
│   ├── Payment bypass via API manipulation
│   └── Coupon code abuse
└── Escalate privileges
    ├── Admin account takeover
    └── SQL injection for direct DB insert
```

---

## 🔗 Complete Security Prompt Chain

```
1️⃣  @threat-modeling-expert
    "Identify threats: STRIDE analysis on registration and payment flows"

2️⃣  @sast-configuration
    "Set up ESLint security + Semgrep + npm audit in CI"

3️⃣  @semgrep-rule-creator
    "Write custom rules for codebase-specific anti-patterns"

4️⃣  @api-security-testing → @sql-injection-testing → @xss-html-injection
    "Manual testing: injection, XSS, IDOR, auth bypass"

5️⃣  @burp-suite-testing
    "Intercept and actively scan with Burp Suite"

6️⃣  @api-fuzzing-bug-bounty
    "Fuzz all endpoints with malformed inputs"

7️⃣  @security/aws-security-audit
    "Cloud security audit: IAM, S3, security groups"

8️⃣  @container-security-hardening
    "Harden Docker images and K8s pod security"

9️⃣  @pci-compliance + @gdpr-data-handling
    "Compliance review for payment and personal data handling"

🔟  @security-audit
    "Final audit report: all findings with severity and remediation"
```
