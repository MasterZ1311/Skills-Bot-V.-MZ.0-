# 🔄 Workflow 01 — Build a Full Stack App from Scratch

This end-to-end workflow walks you through building a production-grade full stack application using the skills library. Each phase has the exact `@skill-name` invocations and what to ask.

---

## 🎯 Goal
Build a complete full stack web application — from idea to deployed production system.

**Example:** College Events Platform (Next.js + Node.js + PostgreSQL)

---

## ⏱️ Estimated Timeline
| Phase | Time |
|---|---|
| Planning | 1-2 hours |
| Database Design | 2-4 hours |
| Backend API | 1-3 days |
| Frontend | 2-4 days |
| Auth & Testing | 1-2 days |
| Deploy | 2-4 hours |
| **Total** | **~1-2 weeks** |

---

## Phase 1 — Planning & Architecture

### Step 1.1 — Feature Planning
```
@brainstorming I want to build a college events platform. 
Students can discover events, register, and get QR tickets.
Organizers can create events, track attendance, and see analytics.
Admins manage the platform.

Help me:
1. Define the core feature set for an MVP
2. Identify user personas and their key jobs to be done
3. List technical risks and unknowns
4. Suggest what to build first and what to defer
```

**Output:** Feature list, user stories, risk map, MVP scope

### Step 1.2 — System Architecture
```
@architecture Design the system architecture for the events platform.
MVP scope: [paste output from Step 1.1]
Tech stack preferences: Next.js 15, Node.js/Express, PostgreSQL, Redis, Vercel

Include:
- Service diagram (components and how they connect)
- Data flow (request → API → DB → response)
- Caching strategy
- Authentication approach
- Third-party services needed (Stripe, email, etc.)
- What runs server-side vs edge vs client
```

**Output:** Architecture diagram (text), component list, tech decision rationale

### Step 1.3 — Codebase Structure
```
@codebase-design Plan the folder structure for our Next.js 15 monorepo:
- apps/web (Next.js frontend + API routes)
- packages/db (Prisma schema and migrations)
- packages/types (shared TypeScript types)
- packages/email (email templates)

Follow: feature-based organization (not layer-based).
Show the full tree with a short description of what goes in each folder.
```

**Output:** Annotated folder tree, module responsibilities

---

## Phase 2 — Database Design

### Step 2.1 — Schema Design
```
@database-design Design the PostgreSQL schema for the events platform.
Entities from our architecture:
- Users (students, organizers, admins)
- Events (with capacity, date, venue, category, status)
- Registrations (connects users to events, with status and payment)
- Tickets (issued after confirmed registration)
- Waitlist (for sold-out events)
- Payments (Stripe payment records)

For each table: columns, data types, constraints, indexes.
Consider: soft deletes, audit timestamps, denormalized counters for performance.
Output: Prisma schema format.
```

**Output:** Complete Prisma schema

### Step 2.2 — ORM Setup
```
@prisma-expert Set up Prisma for our events platform:
1. Configure schema.prisma with our PostgreSQL connection
2. Generate the migration for the schema from Step 2.1
3. Create a seed script with realistic test data (5 events, 10 users, 20 registrations)
4. Write the 5 most critical queries we'll need:
   - Paginated events list with filter + registration count
   - Atomic registration creation + capacity decrement
   - User's registrations with event details
   - Admin dashboard stats (total events, total revenue, registrations today)
   - Check capacity before registration
```

**Output:** Prisma config, migration files, seed.ts, query examples

---

## Phase 3 — Backend API

### Step 3.1 — API Contract
```
@api-design-principles Design the REST API for our events platform.
Resources: /events, /registrations, /tickets, /users (admin), /auth

For each endpoint:
- HTTP method + path
- Auth required? (public / student / organizer / admin)
- Request body shape
- Response body shape
- Error cases and their HTTP codes

Also define:
- Versioning strategy (URL prefix /api/v1/)
- Pagination format (cursor-based)
- Error response format (RFC 7807)
- Rate limiting strategy
```

**Output:** API contract document

### Step 3.2 — Service Implementation
```
@backend-architect Implement the RegistrationService:
Core method: async createRegistration(userId, eventId, paymentMethodId)

Must:
1. Check event exists and is published
2. Check capacity available (with row-level lock to prevent race condition)
3. Create Stripe PaymentIntent
4. Create registration record in PENDING state
5. Confirm payment via Stripe
6. Update registration to CONFIRMED
7. Generate ticket
8. Queue email confirmation (async, don't block response)
9. Return {registration, ticket} on success
10. Rollback all DB changes if any step fails

Use: Prisma transaction + compensating actions for Stripe.
```

**Output:** RegistrationService.ts with full implementation

### Step 3.3 — Authentication
```
@auth-implementation-patterns Implement auth for our events platform:
- Students: sign in with Google (OAuth) — email must be @college.edu
- Organizers: email + password (hashed with argon2)
- Admins: same as organizers but seeded manually
- Sessions: JWT access token (15min) + refresh token (7 days, httpOnly cookie)
- Middleware: verify JWT, attach user to request
- Route protection: decorated with role requirements

Using Next.js: show both the API route handlers and the middleware.ts for client-side route protection.
```

**Output:** Auth implementation, middleware, JWT utilities

### Step 3.4 — Error Handling
```
@error-handling-patterns Implement consistent error handling across our Express/Next.js API:
- Validation errors (400): Zod parse errors → field-level details
- Authentication errors (401): expired token, invalid token
- Authorization errors (403): insufficient role
- Business rule violations (409/422): event full, already registered, payment failed
- Not found (404): event/user/ticket doesn't exist
- Unexpected errors (500): logged, generic message to client

Global error handler middleware.
Never expose stack traces or internal details to clients.
Log all 5xx errors to our monitoring service.
```

**Output:** Error handler middleware, error classes, logging setup

---

## Phase 4 — Frontend

### Step 4.1 — Design Direction
```
@design-it/glassmorphism Design the visual style for our events platform:
Apply glassmorphism aesthetic to:
- Events listing page (cards with glass effect)
- Event detail page (hero + frosted info card)
- Registration modal
- Ticket confirmation screen

Color palette: university purple + gold accents
Typography: Inter (headings) + Inter (body)
Output: CSS custom properties + key component designs in HTML/CSS.
```

**Output:** Design tokens, key component HTML/CSS

### Step 4.2 — Component Library
```
@react-best-practices + @shadcn Build our core component library:
Using: shadcn/ui as base, customized to our design tokens

Components to build:
1. EventCard: image, title, date, venue, capacity badge, register button
2. EventCardSkeleton: loading state
3. RegistrationModal: form + payment + success state
4. TicketCard: QR code + event details, downloadable
5. CapacityBadge: shows spots remaining with urgency colors

Each component: TypeScript props, dark mode support, accessible.
```

**Output:** Component files with full implementation

### Step 4.3 — Pages & Data Fetching
```
@nextjs-app-router-patterns Implement the events listing page:
Route: /events
- Server component: fetch initial events list (SSR for SEO)
- Search params: ?category=music&date=2025-12&page=1
- Suspense: show skeleton while loading more
- Client: filter UI updates URL params → revalidates server component
- Infinite scroll: "Load more" button or automatic on scroll
- SEO: generateMetadata for category pages
```

**Output:** page.tsx, EventsList.tsx, EventFilters.tsx

### Step 4.4 — State & Data Fetching
```
@tanstack-query-expert Set up TanStack Query for client-side data:
- Query: events list (5min stale, background refetch)
- Query: single event (deduplicated across all EventCards)
- Mutation: registerForEvent with:
  - Optimistic: immediately show "Registered" state
  - Success: show ticket modal
  - Error: revert + show error toast
- Prefetch: on EventCard hover, prefetch event detail
```

**Output:** query hooks, mutation hooks, QueryClient config

---

## Phase 5 — Testing

### Step 5.1 — Unit Tests
```
@jest-skill Write unit tests for RegistrationService:
- Happy path: successful registration with payment
- At capacity: event full → CapacityExceededError
- Already registered: duplicate → AlreadyRegisteredError
- Payment failure: Stripe error → registration rolled back, no ticket created
- Concurrent: simulate 2 requests for the last spot → only 1 succeeds

Mock: Prisma (prisma-mock), Stripe SDK, email queue.
Coverage: 100% on createRegistration method.
```

### Step 5.2 — E2E Tests
```
@playwright-skill Write E2E tests for critical user flows:

Test 1: Complete registration flow
1. Visit /events
2. Click first event card
3. Click "Register Now"
4. Fill name + email in modal
5. Enter Stripe test card (4242 4242 4242 4242)
6. Click "Complete Registration"
7. Assert: success screen with QR ticket
8. Assert: ticket accessible at /tickets/:id

Test 2: Sold out event shows waitlist
Test 3: Admin can see registration in dashboard
Test 4: QR ticket can be downloaded as PDF
```

---

## Phase 6 — Deploy

### Step 6.1 — Production Setup
```
@deploy-to-vercel Configure production deployment for our Next.js app:
- Environment variables: DATABASE_URL, REDIS_URL, STRIPE_SECRET, JWT_SECRET, etc.
- Preview deployments: every PR gets a unique URL
- Production domain: events.college.edu
- Edge runtime for middleware (auth checks)
- Database: Neon PostgreSQL (serverless, scales to zero in dev)
- Redis: Upstash (serverless Redis for sessions)
- Static assets: Vercel's edge CDN

GitHub Actions → Vercel: auto-deploy on push to main.
```

### Step 6.2 — Monitoring
```
@prometheus-configuration + @grafana-dashboards Set up monitoring:
Track:
- API response times (p50, p95, p99)
- Error rate by endpoint
- Registration success/failure rate
- Active concurrent users

Alert on:
- p99 latency > 2s
- Error rate > 1%
- Payment failure rate > 5%

Dashboard: "Events Platform Overview" — live during launch.
```

---

## ✅ Launch Checklist

```
□ All E2E tests passing
□ Load test: 100 concurrent registrations (no errors)
□ Security: @security-auditor review completed
□ Accessibility: @accesslint-audit passing (WCAG AA)
□ SEO: @seo-technical audit — event pages indexed
□ Schema markup: @schema-markup-generator applied
□ Error monitoring: Sentry configured
□ Uptime monitoring: Better Uptime pinging /health
□ Backup: automated daily database backup
□ DNS: domain pointed to Vercel
□ SSL: HTTPS confirmed
□ Team notified: support team briefed on launch
```
