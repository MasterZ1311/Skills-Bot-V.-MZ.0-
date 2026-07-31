# 👨‍💻 Full Stack Developer — Skills Guide

Full stack developers bridge the entire application from database schema to deployed UI. This guide covers every skill relevant to your workflow, organized by concern, with a complete prompt chain at the end.

---

## 🗺️ Your Skill Map at a Glance

| Phase | Top Skills |
|---|---|
| Planning | `@brainstorming`, `@architecture`, `@before-you-build`, `@domain-driven-design` |
| Backend | `@backend-architect`, `@api-design-principles`, `@fastapi-pro`, `@nestjs-expert` |
| Database | `@database-design`, `@prisma-expert`, `@supabase`, `@neon-postgres` |
| Frontend | `@react-best-practices`, `@nextjs-best-practices`, `@react-state-management` |
| Auth | `@auth-implementation-patterns`, `@clerk-auth`, `@nextjs-supabase-auth` |
| Testing | `@tdd`, `@e2e-testing`, `@jest-skill`, `@playwright-skill` |
| Code Quality | `@clean-code`, `@code-review-excellence`, `@code-refactoring-refactor-clean` |
| Git & PRs | `@git-advanced-workflows`, `@create-pr`, `@commit` |
| Deploy | `@deploy-to-vercel`, `@firebase`, `@vercel-deployment` |

---

## 📐 Phase 1 — Planning & Architecture

### `@brainstorming`
**When to use:** Before writing a single line of code. Use it to decompose requirements, identify edge cases, and plan your approach.
```
@brainstorming I need to build a college event management platform.
Students can browse events, register, pay, and get QR tickets.
Admins can create events, track attendance, and generate reports.
Help me plan the feature set, technical approach, and what to build first.
```

### `@architecture`
**When to use:** After brainstorming, when you need to design the system's component structure.
```
@architecture Design the system architecture for a college events platform.
Tech stack: Next.js 15, Node.js, PostgreSQL, Redis for sessions.
Include: service boundaries, API layer, data flow, caching strategy, and deployment topology.
```

### `@domain-driven-design` / `@ddd-strategic-design`
**When to use:** For complex domains with multiple business concepts. Helps establish bounded contexts and ubiquitous language.
```
@ddd-strategic-design Map out the domain model for a multi-tenant event platform.
Identify bounded contexts, aggregates, entities, and value objects.
Domains include: Users, Events, Registrations, Payments, Notifications.
```

### `@before-you-build`
**When to use:** A checklist-style skill that forces you to answer critical questions before coding.
```
@before-you-build I'm about to build the payment integration for our events platform.
Run through the pre-build checklist: requirements clarity, edge cases, dependencies,
security considerations, and rollback plan.
```

### `@codebase-design`
**When to use:** When planning the folder structure and code organization of a new project.
```
@codebase-design Plan the folder structure for a Next.js 15 monorepo with:
- apps/web (Next.js frontend)
- apps/api (Express API)
- packages/db (Prisma schema)
- packages/types (shared TypeScript types)
Follow feature-based organization, not layer-based.
```

---

## ⚙️ Phase 2 — Backend Development

### `@backend-architect`
**When to use:** Designing the service layer, defining business logic separation, and making framework-level decisions.
```
@backend-architect Design the service architecture for event registration.
Requirements: handle concurrent registrations, enforce capacity limits,
process payments atomically, send confirmation emails async.
Recommend patterns: CQRS, saga, or simple transactional?
```

### `@api-design-principles`
**When to use:** Before building any API endpoint. Ensures consistent, RESTful, versioned APIs.
```
@api-design-principles Design the REST API contract for:
- Event CRUD (admin only)
- Event discovery with filtering/pagination (public)
- Registration with payment intent creation
- QR ticket validation (scanning)
Output: OpenAPI-style endpoint definitions with request/response shapes.
```

### `@api-endpoint-builder`
**When to use:** When you need to actually implement specific endpoints with validation, error handling, and middleware.
```
@api-endpoint-builder Implement POST /api/events/:id/register
Stack: Node.js, Express, Prisma, Stripe.
Include: auth middleware, capacity check, Stripe payment intent, DB transaction,
email queue dispatch. Full implementation with error handling.
```

### `@nestjs-expert` / `@fastapi-pro` / `@nodejs-best-practices`
**When to use:** Language/framework-specific expert guidance.
```
@nestjs-expert Set up a NestJS module for event registration with:
- RegistrationModule, RegistrationService, RegistrationController
- Guards for authentication and capacity enforcement
- DTOs with class-validator
- Repository pattern with Prisma
```

### `@error-handling-patterns`
**When to use:** After implementing business logic, before testing.
```
@error-handling-patterns Review the registration service and implement
a consistent error handling strategy. Distinguish between:
validation errors (400), business rule violations (422),
auth errors (401/403), and unexpected failures (500).
```

### `@microservices-patterns`
**When to use:** When your app needs to scale to separate services.
```
@microservices-patterns Our events platform is growing. Plan how to extract
the notification service into its own microservice.
Define: message contracts, async communication via queues,
service discovery, and how to handle partial failures.
```

---

## 🗄️ Phase 3 — Database

### `@database-design`
**When to use:** When designing your data model from scratch.
```
@database-design Design the PostgreSQL schema for a college events platform.
Entities: users, events, registrations, payments, tickets, notifications.
Include: indexes, constraints, foreign keys, soft deletes where needed.
Output as Prisma schema format.
```

### `@prisma-expert`
**When to use:** All Prisma-specific work: schema design, migrations, queries, relations.
```
@prisma-expert Write the Prisma queries for:
1. Paginated event listing with filters (category, date, available spots)
2. Atomic registration: create registration + decrement capacity + create payment record
3. Admin dashboard: events with registration counts and revenue totals
Use transactions where needed. Optimize for N+1 avoidance.
```

### `@drizzle-orm-expert`
**When to use:** If your stack uses Drizzle instead of Prisma.

### `@supabase` / `@neon-postgres`
**When to use:** When using these managed Postgres platforms with their specific features (RLS, edge functions, branching).
```
@supabase Set up Row Level Security policies for our events platform:
- Public users can only read published events
- Students can only read/update their own registrations
- Admins can read/write everything
Write the RLS policies in SQL.
```

### `@database-migrations-sql-migrations`
**When to use:** When creating or running migrations safely.
```
@database-migrations-sql-migrations We need to add a `waitlist` feature.
New column: events.waitlist_enabled (boolean), new table: waitlist_entries.
Write a safe migration that:
- Is reversible
- Handles existing data
- Adds appropriate indexes
```

---

## 🎨 Phase 4 — Frontend Development

### `@react-best-practices`
**When to use:** Any React component design decision, performance considerations, or anti-pattern detection.
```
@react-best-practices Review this EventCard component.
Check for: unnecessary re-renders, missing memoization, prop drilling,
incorrect useEffect usage, and accessibility issues.
```

### `@nextjs-best-practices` / `@nextjs-app-router-patterns`
**When to use:** Next.js App Router specific patterns — server components, data fetching, caching, routing.
```
@nextjs-app-router-patterns Implement the events listing page using App Router.
Use: server component for data fetching, Suspense for loading state,
parallel routes for modal-style event detail, ISR with revalidation.
```

### `@react-state-management` / `@zustand-store-ts` / `@tanstack-query-expert`
**When to use:** Choosing and implementing state management.
```
@tanstack-query-expert Set up React Query for:
- Event listing with infinite scroll pagination
- Registration mutation with optimistic updates
- Real-time ticket status polling
Include: query keys strategy, error boundaries, loading states.
```

### `@frontend-api-integration-patterns`
**When to use:** Building the client-side API layer.
```
@frontend-api-integration-patterns Create a typed API client for our events platform.
Use: fetch with type-safe wrappers, error handling, auth token injection,
request/response interceptors. TypeScript interfaces matching our OpenAPI spec.
```

### `@react-ui-patterns`
**When to use:** Building complex UI components with correct accessibility and interaction patterns.
```
@react-ui-patterns Build an EventRegistrationModal that:
- Shows event details, pricing, capacity
- Handles seat selection
- Shows Stripe payment form
- Displays success state with QR ticket
Follow compound component pattern. Fully accessible (ARIA, focus management).
```

---

## 🔐 Phase 5 — Authentication

### `@auth-implementation-patterns`
**When to use:** Designing the auth strategy before implementing.
```
@auth-implementation-patterns Design the auth system for our events platform:
- Students login with college email (OAuth via Google)
- Admins have username/password
- API uses JWT with refresh tokens
- Next.js middleware for route protection
Recommend: Clerk, NextAuth, or custom? Explain tradeoffs.
```

### `@clerk-auth` / `@nextjs-supabase-auth`
**When to use:** Implementing with a specific auth provider.
```
@clerk-auth Integrate Clerk auth into our Next.js 15 App Router project.
Set up: sign-in/sign-up pages, middleware for protected routes,
user metadata for role-based access (student/admin),
webhook to sync users to our Postgres database.
```

---

## ✅ Phase 6 — Testing

### `@tdd` / `@tdd-workflow`
**When to use:** Before implementing a complex feature. Write tests first.
```
@tdd-workflow Walk me through TDD for the event registration service.
Start with the red phase: write failing tests for:
- Successful registration
- Registration when event is full
- Duplicate registration prevention
- Payment failure rollback
```

### `@jest-skill` / `@vitest-skill`
**When to use:** Unit and integration test implementation.
```
@jest-skill Write comprehensive tests for the RegistrationService.
Include: happy paths, edge cases, mock Prisma and Stripe,
test the atomic transaction behavior.
```

### `@playwright-skill` / `@cypress-skill`
**When to use:** End-to-end testing for complete user flows.
```
@playwright-skill Write E2E tests for the full registration flow:
1. Student visits events page
2. Clicks "Register" on an event
3. Completes Stripe payment (use test card)
4. Sees confirmation and QR ticket
5. Admin sees registration in dashboard
```

### `@e2e-testing-patterns`
**When to use:** Structuring your entire E2E test suite.

---

## 🔍 Phase 7 — Code Quality & Review

### `@clean-code`
**When to use:** After implementing a feature to refactor and clean up.
```
@clean-code Review the EventService class (pasted below).
Apply Clean Code principles: extract methods, rename for clarity,
remove magic numbers, simplify conditionals. Show the refactored version.
```

### `@code-review-excellence`
**When to use:** Doing a thorough code review before merging.
```
@code-review-excellence Review this PR that adds the waitlist feature.
Check: correctness, security, performance, maintainability, test coverage.
Format feedback as: [BLOCKING], [SUGGESTION], [NITPICK].
```

### `@code-refactoring-tech-debt`
**When to use:** Tackling accumulated technical debt.

### `@lint-and-validate`
**When to use:** Quick automated quality check before committing.
```
@lint-and-validate Run quality checks on the codebase:
ESLint config recommendations for Next.js + TypeScript,
Prettier formatting rules, pre-commit hooks with Husky.
```

---

## 🚀 Phase 8 — Git & Deployment

### `@create-pr`
**When to use:** Preparing a clean, well-documented pull request.
```
@create-pr Create a PR description for the waitlist feature.
Changes include: DB migration, service layer, API endpoint, UI component, tests.
Format: Summary, Changes Made, Testing Done, Screenshots, Breaking Changes.
```

### `@git-advanced-workflows`
**When to use:** Complex git operations — rebasing, cherry-picking, stash management.

### `@deploy-to-vercel` / `@vercel-deployment`
**When to use:** Deploying Next.js to Vercel.
```
@deploy-to-vercel Set up production deployment for our Next.js events platform:
- Environment variables management
- Preview deployments for PRs
- Production domain setup
- Edge functions configuration
- Database connection pooling for serverless
```

---

## 🔗 Complete Prompt Chain — Build a Full Stack Feature

Here is the exact sequence to build the **Event Registration** feature end-to-end:

```
1️⃣  @brainstorming
    "Plan the event registration feature: student flow, admin flow, edge cases"

2️⃣  @database-design
    "Design the Prisma schema: registrations, payments, capacity tracking"

3️⃣  @api-design-principles
    "Design REST endpoints for registration, payment intent, ticket retrieval"

4️⃣  @backend-architect
    "Design the RegistrationService with atomic transaction pattern"

5️⃣  @nestjs-expert (or @fastapi-pro / @nodejs-best-practices)
    "Implement the registration endpoint with validation and Stripe integration"

6️⃣  @auth-implementation-patterns
    "Add JWT middleware and role-based access control"

7️⃣  @react-best-practices + @nextjs-app-router-patterns
    "Build the registration modal and event detail page"

8️⃣  @tanstack-query-expert
    "Set up mutations with optimistic updates for registration"

9️⃣  @tdd → @jest-skill → @playwright-skill
    "Write unit tests, integration tests, and E2E test for the full flow"

🔟  @clean-code → @code-review-excellence
    "Refactor and do final review"

1️⃣1️⃣  @create-pr → @deploy-to-vercel
    "Create PR and deploy to production"
```

---

## 💡 Pro Tips for Full Stack Developers

1. **Always start with `@brainstorming`** — it prevents building the wrong thing
2. **Use `@database-design` before any other code** — schema is hardest to change later
3. **`@api-design-principles` before implementation** — agree on contracts first
4. **Chain `@tdd` + `@jest-skill`** for complex business logic — write failing tests before code
5. **`@code-review-excellence` is not just for reviewing others** — use it on your own code
