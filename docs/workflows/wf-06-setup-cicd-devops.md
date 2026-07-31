# 🚀 Workflow 06 — Set Up CI/CD & DevOps Pipeline

This workflow builds a complete CI/CD pipeline from scratch — automated testing, security scanning, infrastructure as code, staging/production environments, and observability.

---

## 🎯 Goal
Ship code to production confidently and quickly — with automated quality gates, zero-downtime deployments, and full observability.

**Example:** CI/CD pipeline for the college events platform

---

## ⏱️ Timeline

| Phase | Time |
|---|---|
| Source Control Strategy | 2-4 hours |
| CI Pipeline (GitHub Actions) | Half a day |
| Container & IaC | 1 day |
| Environments | Half a day |
| CD (Deployment) | Half a day |
| Observability | 1 day |
| Alerts & On-call | Half a day |
| **Total** | **~3-4 days** |

---

## Phase 1 — Source Control Strategy

### Step 1.1 — Git Workflow
```
@git-branching-strategy Design our git branching strategy:
Team size: 3 engineers
Release cadence: deploy multiple times per day

Recommend: GitHub Flow (simpler than GitFlow for our team size)

Branch rules:
- main: production-deployed, always green, protected
- feature/*: developer branches, opened as PR
- hotfix/*: emergency fixes, can skip staging (with approval)

PR rules:
- Must pass: all CI checks
- Requires: 1 approving review from another engineer
- Auto-delete: head branch after merge
- Squash: all feature PRs squash-merged (clean history)

Commit convention: Conventional Commits
- feat: new feature
- fix: bug fix
- perf: performance improvement
- chore: maintenance, dependencies
- docs: documentation only
Used for: auto-changelog generation
```

### Step 1.2 — Branch Protection
```
@github-automation Configure GitHub branch protection:
Branch: main

Protection rules:
- Require pull request: minimum 1 review
- Dismiss stale reviews: when new commits pushed
- Require status checks to pass:
  - unit-tests
  - type-check
  - lint
  - security-scan
  - build
- Require linear history (no merge commits)
- Include administrators: yes (no bypasses)

CODEOWNERS:
/infra/ → @devops-team
/packages/db/ → @backend-lead (any DB change needs backend lead review)
/docs/ → any team member
/ → any engineer

Auto-assign reviewers: GitHub auto-assign based on CODEOWNERS.
```

---

## Phase 2 — CI Pipeline

### Step 2.1 — Base CI Workflow
```
@github-actions-expert Build our base CI pipeline in GitHub Actions:
File: .github/workflows/ci.yml
Trigger: push to any branch, PR to main

Jobs (run in parallel where possible):

Job 1: lint-and-type-check (2-3 minutes)
  - actions/checkout@v4
  - actions/setup-node@v4 (Node 20, cache: npm)
  - npm ci
  - npm run lint (ESLint + ESLint security plugin)
  - npm run type-check (tsc --noEmit)

Job 2: unit-tests (3-5 minutes)
  - Same setup
  - npm run test -- --coverage --ci
  - Upload coverage: codecov/codecov-action
  - Fail if coverage drops below 80%

Job 3: build (2-3 minutes)
  - npm run build
  - Upload artifact: .next/ folder (for deployment job)

Job 4: security-scan (2-3 minutes)
  - npm audit --audit-level=high
  - Run Semgrep: semgrep/semgrep-action with our rule sets

All jobs run in parallel. PR blocked if any job fails.
Total CI time target: < 8 minutes.
```

### Step 2.2 — E2E Tests in CI
```
@github-actions-expert Add E2E tests to CI for PRs to main:
File: .github/workflows/e2e.yml
Trigger: PR to main branch only (E2E is slower, not for every push)

Setup:
1. Start test database (PostgreSQL service container)
2. Run migrations on test DB
3. Seed test data
4. Start the Next.js app in test mode
5. Run Playwright tests

Optimization:
- Shard Playwright across 3 workers: --shard=1/3, 2/3, 3/3
- Cache Playwright browsers between runs
- Only run tests affected by changed files (Playwright --project filter)

Reporting:
- GitHub summary with pass/fail count
- Upload: Playwright HTML report as workflow artifact
- Post: link to report as PR comment

Expected time: 8-12 minutes with sharding.
```

### Step 2.3 — Dependency Updates
```
@github-automation Set up automated dependency updates:
Tool: Dependabot

.github/dependabot.yml:
- Package manager: npm
- Schedule: weekly (Monday)
- Groups: group all minor + patch updates into one PR
- Separate PRs for: major version bumps
- Auto-merge: patch updates that pass CI (no human review needed)
- Labels: dependencies, automated
- Assignees: @backend-lead for review

Also: security updates (Dependabot security alerts)
- Auto-create PR for any CVE
- Auto-merge if: only dev dependency + passes CI
- Require review if: production dependency
```

---

## Phase 3 — Containers & Infrastructure as Code

### Step 3.1 — Dockerfile
```
@docker-multi-stage Build a production Dockerfile:

# Multi-stage build for minimal image size
FROM node:20-alpine AS base
WORKDIR /app
COPY package*.json ./

FROM base AS deps
RUN npm ci --only=production

FROM base AS dev-deps
RUN npm ci

FROM dev-deps AS builder
COPY . .
RUN npm run build

# Production image
FROM node:20-alpine AS runner
WORKDIR /app

# Security: non-root user
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# Copy only production artifacts
COPY --from=deps /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./

USER nextjs

EXPOSE 3000
ENV PORT 3000

CMD ["node_modules/.bin/next", "start"]

# Final image size target: < 300MB
# RUN: trivy image to scan for CVEs before push
```

### Step 3.2 — Terraform Infrastructure
```
@terraform-expert Write Terraform for our events platform infra:
Provider: AWS

Resources:
1. Networking:
   - VPC: 10.0.0.0/16
   - Public subnets (2 AZs): for ALB
   - Private subnets (2 AZs): for ECS tasks + RDS
   - NAT Gateway: for private subnet outbound
   - Security Groups: ALB (80/443 open), ECS (only from ALB), RDS (only from ECS)

2. Database:
   - aws_db_instance: PostgreSQL 15, t3.medium, Multi-AZ for prod
   - aws_db_subnet_group: private subnets only
   - aws_db_parameter_group: enable pgvector extension

3. Compute:
   - aws_ecs_cluster: events-platform
   - aws_ecs_task_definition: API service (512 CPU, 1024 MB)
   - aws_ecs_service: 2 desired count, ALB attached, auto-scaling

4. Load Balancer:
   - aws_lb: ALB in public subnets
   - aws_lb_listener: HTTPS (443) with ACM cert, redirect HTTP→HTTPS
   - aws_lb_target_group: forward to ECS service

5. Container Registry:
   - aws_ecr_repository: events-api (lifecycle: keep last 10 images)

Separate workspaces: staging, production
Variables: environment, instance_type, desired_count
```

### Step 3.3 — Terraform State Management
```
@terraform-expert Set up Terraform remote state:
Backend: S3 + DynamoDB locking

S3 bucket: events-platform-tfstate
- Versioning: enabled (rollback if state corrupted)
- Encryption: SSE-S3
- Block public access: all blocked

DynamoDB table: events-platform-tf-locks
- Hash key: LockID (String)

backend.tf:
terraform {
  backend "s3" {
    bucket         = "events-platform-tfstate"
    key            = "production/terraform.tfstate"
    region         = "ap-south-2"
    encrypt        = true
    dynamodb_table = "events-platform-tf-locks"
  }
}

GitHub Actions: use OIDC (not long-lived keys) to authenticate to AWS.
Workflow: terraform plan on PR → terraform apply on merge to main.
```

---

## Phase 4 — Environments

### Step 4.1 — Environment Strategy
```
@environment-configuration Design our environment strategy:

3 environments:
1. Development (local)
   - Database: Docker Compose PostgreSQL
   - All external services: mocked or sandboxed (Stripe test mode)
   - .env.local (gitignored)

2. Staging (auto-deployed from main branch)
   - URL: staging.events.college.edu
   - Database: separate RDS instance (small: t3.micro)
   - Stripe: test mode keys
   - Email: sent to Mailtrap (never real users)
   - Auto-seeded: test data refreshed weekly
   - Access: team only (IP allowlist or VPN)

3. Production
   - URL: events.college.edu
   - Database: RDS Multi-AZ (t3.medium)
   - Stripe: live keys
   - Email: real delivery (Resend/SES)
   - Deploy: manual trigger after staging validated

Environment variables management:
- Local: .env.local (gitignored)
- Staging/Prod: AWS Secrets Manager (never in GitHub)
- GitHub Actions: uses OIDC to pull from Secrets Manager (no stored secrets)
```

### Step 4.2 — Neon Database Branching (Alternative)
```
@supabase Design database branching workflow using Neon:
Neon supports git-like database branches — perfect for preview environments

Workflow:
- main branch → production database
- staging branch → staging database
- PR branch → ephemeral DB branch (auto-created with Neon GitHub Integration)

PR workflow:
1. Dev opens PR → Neon creates a branch from staging DB (instant, copy-on-write)
2. PR CI: runs migrations on that branch, runs tests against it
3. PR merged → branch deleted automatically
4. Staging deploy: migrations run on staging DB branch

Benefits:
- Each PR has isolated database (no shared staging conflicts)
- Test migrations before they hit staging
- Preview apps test with real-ish data structure

Cost: only pay for storage of diff from parent branch (very cheap).
```

---

## Phase 5 — Continuous Deployment

### Step 5.1 — Staging Auto-Deploy
```
@github-actions-expert Build staging auto-deployment:
File: .github/workflows/deploy-staging.yml
Trigger: push to main (after all CI passes)

Steps:
1. Build Docker image: docker build -t events-api:$GITHUB_SHA .
2. Push to ECR: aws ecr push
3. Run DB migrations on staging:
   npx prisma migrate deploy (against staging DB)
4. Deploy to ECS:
   aws ecs update-service --force-new-deployment
5. Wait for stability:
   aws ecs wait services-stable (timeout: 10 minutes)
6. Health check:
   curl https://staging.events.college.edu/health → assert 200
7. Smoke test:
   run 5 critical Playwright E2E tests against staging
8. Notify: Slack "#deployments" — "✅ Staging deployed: [sha] by [author]"
9. Rollback (if any step fails):
   aws ecs update-service --task-definition previous_task_def
   Notify: Slack "#deployments" — "🔴 Staging deploy failed, rolled back"
```

### Step 5.2 — Production Deployment (Manual Gate)
```
@github-actions-expert Build production deployment with manual approval:
File: .github/workflows/deploy-production.yml
Trigger: manual dispatch (workflow_dispatch) with input: version tag

GitHub Environment: "production"
- Required reviewers: 2 engineers must approve
- Wait timer: 30 minutes (cooling off period)

Steps after approval:
1. Pull image from ECR (already built, don't rebuild)
2. Run DB migrations on production:
   - Dry-run first: prisma migrate status
   - Apply: prisma migrate deploy
3. Blue/Green deploy:
   - Create new ECS task definition (new image)
   - Update service with deployment circuit breaker
   - ECS shifts traffic to new tasks
   - Old tasks drained and removed (zero downtime)
4. Health check: 3 retries over 5 minutes
5. Notify: Slack + email to team — "🟢 Production deployed v1.2.3"
6. Tag release: git tag v1.2.3, create GitHub Release with changelog

Rollback:
- Automatic: ECS circuit breaker triggers if new tasks fail health checks
- Manual: re-run workflow with previous tag
```

### Step 5.3 — Database Migration Safety
```
@database-migrations-patterns Safe database migration strategy:
Rules for zero-downtime migrations:

✅ SAFE (can run with old code running):
- Add new column (with default or nullable)
- Add new table
- Add index CONCURRENTLY
- Add constraint if already satisfied

⚠️  MULTI-PHASE (requires deploy coordination):
- Rename column:
  Phase 1: Add new column, write to both
  Phase 2: Deploy code using new column only
  Phase 3: Drop old column
- Remove column:
  Phase 1: Deploy code that doesn't use the column
  Phase 2: Run migration to drop column

❌ NEVER (causes downtime):
- Add NOT NULL column without default on large table
- Lock the entire table with exclusive lock
- Rename table referenced by code

Tool: prisma migrate deploy (runs on deploy, never on startup)
CI check: if migration contains DROP TABLE or DROP COLUMN → require 2 approvals.
```

---

## Phase 6 — Observability

### Step 6.1 — Metrics & Dashboards
```
@prometheus-configuration + @grafana-dashboards Set up metrics:

Prometheus scrape targets:
- Node.js app: expose /metrics (prom-client library)
  - http_request_duration_seconds (histogram, labeled by method, route, status)
  - http_requests_total (counter)
  - registration_created_total (custom business metric)
  - payment_processed_total (with success/fail label)
  - active_connections (gauge)

Grafana Dashboards:

Dashboard 1: "Platform Overview" (for on-call engineer)
  Row 1: Traffic — RPS, error rate, p50/p95/p99 latency
  Row 2: Business — registrations/min, payments/min, revenue today
  Row 3: Infrastructure — CPU, memory, DB connections, DB query time

Dashboard 2: "Registration Funnel" (for PM)
  - Events viewed today → Registration started → Payment submitted → Confirmed
  - Drop-off at each stage
  - Conversion rate trend (7-day)

Dashboard 3: "Database Health"
  - Query latency percentiles
  - Connection pool utilization
  - Slow queries (> 100ms)
  - Replication lag (if Multi-AZ)
```

### Step 6.2 — Logging
```
@logging-observability Set up structured logging:
Library: pino (fastest Node.js logger)

Log format: JSON, always structured
{
  "timestamp": "2025-12-15T12:30:45.123Z",
  "level": "info",
  "service": "events-api",
  "version": "1.2.3",
  "requestId": "req_abc123",  // propagated from X-Request-ID header
  "userId": "user_xyz",        // added by auth middleware
  "method": "POST",
  "path": "/api/registrations",
  "statusCode": 201,
  "durationMs": 245,
  "message": "Registration created"
}

Log levels:
- ERROR: unexpected errors → always alert
- WARN: expected errors (capacity full, payment failed) → daily review
- INFO: all requests, key business events → queryable
- DEBUG: detailed trace → only in development

Log shipping: Pino → stdout → AWS CloudWatch Logs
Log querying: CloudWatch Insights (or OpenSearch if scale demands)
Retention: 90 days (then archive to S3 Glacier)
```

### Step 6.3 — Error Tracking
```
@sentry-configuration Set up Sentry for error tracking:
Projects: events-api (backend), events-web (frontend)

Source maps: upload on every deploy for human-readable stack traces

Sentry config (Next.js):
withSentryConfig({
  tracesSampleRate: 0.1,          // 10% of transactions
  profilesSampleRate: 0.1,
  replaysSessionSampleRate: 0.01, // 1% of sessions
  replaysOnErrorSampleRate: 1.0,  // 100% with errors
})

Error grouping:
- Add user context (id, college) to Sentry events → "which users affected?"
- Add release tag (git sha) → "introduced in which release?"
- Ignore: expected errors (401, 403, 404) — not bugs

Alerts from Sentry:
- New issue: Slack notification
- Error rate spike (> 10x baseline): PagerDuty page
- Performance regression (P95 > 2s): Slack warning
```

---

## Phase 7 — Alerting & On-Call

### Step 7.1 — Alert Definitions
```
@prometheus-configuration Define our alerting rules:
File: alerts.yaml

Critical (page immediately):
- API error rate > 5% for 5 minutes
- API P99 latency > 5 seconds for 5 minutes
- Database connection pool > 90% full
- Payment error rate > 10% for 2 minutes

Warning (Slack notification, no page):
- API error rate > 1% for 10 minutes
- API P99 latency > 2 seconds for 10 minutes
- Database slow queries > 5 per minute
- Disk usage > 80%
- Memory usage > 85%

Business alerts (Slack, business hours only):
- 0 registrations in 30 minutes during peak hours (possible checkout bug)
- 0 events created in 24 hours (organizer experience issue)
- Stripe webhook failures > 3 in 1 hour

Route: PagerDuty → on-call rotation (7-day rotation, 2 engineers)
```

### Step 7.2 — Runbooks
```
@wiki-builder Write runbooks for our top 5 incident scenarios:

Runbook 1: API is returning 500 errors
1. Check Sentry: what error? Which endpoint? How many users affected?
2. Check CloudWatch Logs: search for ERROR in last 15 minutes
3. Check deployment: was there a recent deploy? → roll back if yes
4. Check database: is it reachable? (aws rds describe-db-instances → status)
5. Restart ECS service if no clear cause
6. Update status page (statuspage.io)
7. Postmortem: after resolution, document root cause

Runbook 2: Payments failing
Runbook 3: Database connection pool exhausted
Runbook 4: Staging deploy failed, need to unblock main
Runbook 5: SSL certificate expiring
```

---

## ✅ CI/CD Pipeline Completion Checklist

```
Source Control:
□ Branch protection on main: reviews + CI required
□ CODEOWNERS configured
□ Dependabot: weekly dependency updates + security alerts

CI Pipeline:
□ Lint + type-check job: < 3 minutes
□ Unit tests job: < 5 minutes, coverage reported
□ Security scan: Semgrep + npm audit
□ E2E tests on main PRs: < 12 minutes

Infrastructure:
□ Terraform: all infra as code, remote state in S3
□ Environments: staging + production separated
□ Secrets: all in AWS Secrets Manager (nothing in git)
□ Docker: non-root user, scanned with Trivy

Deployment:
□ Staging: auto-deployed on every merge to main
□ Production: manual approval gate (2 approvers)
□ Rollback: tested and works (deployed old version successfully)
□ DB migrations: zero-downtime strategy documented

Observability:
□ Metrics: Prometheus + Grafana dashboards live
□ Logging: structured JSON logs in CloudWatch
□ Error tracking: Sentry active for all environments
□ Uptime monitoring: pinging /health every 60 seconds

Alerting:
□ PagerDuty: on-call rotation configured
□ Critical alerts: tested (deliberately triggered and fired)
□ Runbooks: written for top 5 incident types
□ Status page: configured (statuspage.io or Instatus)
```
