# ✍️ Technical Writer — Skills Guide

Technical writers create documentation that helps developers, users, and stakeholders understand and use software effectively. This guide covers API docs, READMEs, wikis, tutorials, code documentation, changelog automation, and writing quality.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| API Documentation | `@api-documentation`, `@api-documentation-generator`, `@openapi-spec-generation` |
| README & Project Docs | `@readme`, `@docs-architect`, `@documentation-templates` |
| Wiki & Knowledge Base | `@wiki-architect`, `@wiki-builder`, `@wiki-page-writer`, `@wiki-onboarding` |
| Code Documentation | `@code-documentation-doc-generate`, `@documentation-generation-doc-generate` |
| ADRs | `@architecture-decision-records`, `@documentation-and-adrs` |
| Tutorials | `@technical-tutorials`, `@tutorial-engineer`, `@lesson-generator` |
| Writing Quality | `@beautiful-prose`, `@professional-proofreader`, `@copy-editing`, `@avoid-ai-writing`, `@unslop` |
| Changelog | `@changelog-automation`, `@changelog-updates` |

---

## 📄 API Documentation

### `@api-documentation-generator`
Generate comprehensive, developer-friendly API documentation.
```
@api-documentation-generator Document our events platform REST API:

Format: OpenAPI 3.0 → render with Redocly or Swagger UI

Sections to cover:
1. Authentication: Bearer JWT — how to obtain + refresh token
2. Rate limits: 100 req/min per user, 1000 req/min per IP
3. Error codes: standardized format (RFC 7807 Problem Details)
4. Pagination: cursor-based for all list endpoints

Endpoints to document (each with: description, request/response schema, examples, error cases):

Public:
- GET /api/v1/events (list with filters: category, date, capacity)
- GET /api/v1/events/:id (event detail with registration stats)

Student (requires auth):
- POST /api/v1/events/:id/register
- DELETE /api/v1/registrations/:id (cancel)
- GET /api/v1/tickets/:id

Admin (requires admin role):
- POST /api/v1/events (create event)
- PATCH /api/v1/events/:id (update event)
- GET /api/v1/events/:id/registrations (list attendees)

Include: curl examples, JavaScript (fetch) examples, Python (requests) examples.
```

### `@openapi-spec-generation`
Generate OpenAPI 3.0 YAML/JSON from code or description.
```
@openapi-spec-generation Generate the complete OpenAPI 3.0 spec for our registration endpoint:

POST /api/v1/events/{eventId}/register

Spec should include:
- operationId: registerForEvent
- tags: [registrations]
- security: [BearerAuth]
- parameters: eventId (path, required, format: uuid)
- requestBody:
    ticketCount: integer (1-5, required)
    paymentMethodId: string (required for paid events)
    promoCode: string (optional)
- responses:
    201: Registration created (with ticket URL in Location header)
    400: Validation error (missing fields)
    401: Not authenticated
    403: Not a student (admin trying to register)
    409: Event sold out or already registered
    422: Payment processing failed
    429: Rate limit exceeded
- components: Registration schema, PaymentError schema

Include: x-examples with realistic test data.
```

---

## 📖 README & Project Documentation

### `@readme`
Write a README that gets developers up and running fast.
```
@readme Write a README for our events platform backend API:

Sections:
1. Project title + one-line description
2. Tech stack badge row (Node.js, TypeScript, PostgreSQL, Redis, Docker)
3. Features list (bullet points, benefit-focused not feature-focused)
4. Prerequisites: Node 20+, PostgreSQL 15+, Redis 7+, Docker (optional)
5. Quick Start:
   git clone → cd → cp .env.example .env → fill in values → npm install
   → docker compose up db redis → npm run db:migrate → npm run db:seed
   → npm run dev → open http://localhost:3000
6. Environment variables: table with name, description, example, required?
7. Available scripts: npm run dev, build, test, lint, db:migrate, db:seed
8. Project structure: annotated folder tree
9. API documentation link
10. Contributing guide (PR process, branch naming)
11. License

Tone: friendly and welcoming. First-time contributor should be running in < 5 minutes.
```

### `@docs-architect`
Design the overall documentation structure for a project or product.
```
@docs-architect Plan the complete documentation structure for our events platform:

Audience: 
- Developers (API consumers, contributors)
- Event organizers (product users)
- System administrators (self-hosting)
- Students (end users — app help)

Documentation types:
1. Developer Docs (docs.events.college.edu)
   - Getting started (< 5 min quickstart)
   - API reference (auto-generated from OpenAPI)
   - SDKs (JavaScript, Python)
   - Webhooks guide
   - Authentication guide
   - Tutorials (embed events on website, build attendance scanner)

2. Organizer Guide (events.college.edu/help/organizer)
   - Create first event (tutorial)
   - Manage registrations
   - Export attendee list
   - Understanding analytics

3. Admin Guide (internal)
   - Installation and deployment
   - Configuration reference
   - Troubleshooting

Recommend: Docusaurus for developer docs, built-in CMS page for user guides.
```

### `@documentation-templates`
Reusable templates for consistent documentation.
```
@documentation-templates Create documentation templates for our project:

1. Feature documentation template:
   - Overview (1 paragraph)
   - How it works (3-5 bullets or diagram)
   - Configuration options (table)
   - Usage examples (code)
   - Edge cases and limitations
   - Related features (links)

2. Troubleshooting entry template:
   - Problem: [exact error message or symptom]
   - Cause: [why it happens]
   - Solution: [numbered steps]
   - Prevention: [how to avoid it]
   - Related issues: [links]

3. ADR template: [next section]
```

---

## 📚 Wikis & Knowledge Bases

### `@wiki-architect`
Design the structure of a team wiki.
```
@wiki-architect Design a Notion wiki for our engineering team:

Top-level sections:
1. Getting Started → onboarding for new engineers
2. Architecture → system diagrams, tech decisions, ADRs
3. Engineering Practices → coding standards, PR process, deployment
4. Services → per-service documentation (API, Worker, Frontend)
5. Runbooks → step-by-step operational procedures
6. Retrospectives → team retrospective notes
7. Meeting Notes → architecture meetings, planning sessions

For each section:
- What pages live here?
- Who is the owner?
- How often is it reviewed/updated?
- What's the naming convention for pages?
```

### `@wiki-builder`
Rapidly build out a wiki with content.
```
@wiki-builder Build the "Engineering Practices" section of our wiki:

Create pages for:

Page 1: Git Branching Strategy
- Branch naming: feature/*, fix/*, chore/*, release/*
- PR rules: must have 1 approval, CI passing, no conflicts
- Commit messages: Conventional Commits format
- When to squash vs merge commit

Page 2: Code Review Guidelines
- What to look for (correctness, security, performance, readability)
- How to give feedback (constructive language, [BLOCKING] vs [SUGGESTION])
- Response time SLA: 24 hours business hours
- Self-review checklist before requesting review

Page 3: Deployment Process
- Environments: dev → staging → production
- Staging deploy: automatic on merge to main
- Production deploy: manual trigger, requires team lead approval
- Rollback procedure
```

### `@wiki-onboarding`
Write an onboarding guide for new team members.
```
@wiki-onboarding Write an onboarding guide for new engineers joining our team:

Week 1 checklist:
Day 1: accounts setup (GitHub, Slack, Notion, Vercel, AWS)
Day 1: dev environment setup (clone repos, run locally, confirm working)
Day 2: read: architecture overview, ADRs, coding standards
Day 3: complete first task (labeled "good first issue")
Day 4: pair programming session with a team member
Day 5: 1:1 with engineering lead, Q&A

Resources:
- Links to all key documentation
- Who to ask about what (team directory with specialties)
- Slack channels to join

First 30 days: what should they have shipped? What should they understand?
```

---

## 📐 Architecture Decision Records (ADRs)

### `@architecture-decision-records`
Document why architectural decisions were made.
```
@architecture-decision-records Write an ADR for our database selection decision:

ADR-001: Choosing PostgreSQL over MongoDB for the events platform

Context:
We needed a database for storing events, registrations, and payments.
Two options were evaluated: PostgreSQL and MongoDB.

Decision: PostgreSQL

Rationale:
- Our data has strong relational structure (events → registrations → payments)
- ACID transactions critical for atomic registration + payment
- Better tooling: Prisma ORM, pgvector for future semantic search
- Team familiarity with SQL
- Neon provides serverless PostgreSQL with branching for dev workflow

Consequences:
- Schema migrations required (managed with Prisma Migrate)
- Horizontal scaling requires read replicas (vs MongoDB sharding)
- Document-like flexibility traded for relational integrity

Status: Accepted
Date: 2025-01-15
Author: [name]
Reviewers: [names]
```

---

## 📝 Code Documentation

### `@code-documentation-doc-generate`
Generate inline documentation for existing code.
```
@code-documentation-doc-generate Generate JSDoc documentation for this TypeScript code:
[paste RegistrationService class]

For each method generate:
- @description: what the method does
- @param: each parameter with type and description
- @returns: return type and what it represents
- @throws: all possible exceptions and when they're thrown
- @example: usage example with realistic values

Also add class-level JSDoc explaining:
- Purpose of the class
- Dependencies injected
- Usage context (which layer, called by what)
```

---

## 🎓 Tutorials

### `@technical-tutorials`
Write developer tutorials that teach by doing.
```
@technical-tutorials Write a tutorial: "Integrate the Events API in 15 Minutes"

Audience: junior developers who want to embed events data in their college website
Prerequisites: basic JavaScript knowledge, have an API key

Tutorial structure:
1. Introduction (30 seconds to read): what you'll build
2. Get your API key: 3 steps with screenshots
3. List events: curl command first, then JavaScript fetch, then Python requests
4. Filter by category: show query parameter usage
5. Register a student: POST request with auth header
6. Validate a ticket: explain the QR code payload
7. Complete example: embed events widget in 20 lines of vanilla JS

Code: runnable in browser console or simple HTML file — no build step
Each step: brief explanation + code block + expected output
Final: link to CodeSandbox with working demo
```

### `@tutorial-engineer`
Build interactive, engaging tutorials.
```
@tutorial-engineer Design an interactive onboarding tutorial for organizers:
Goal: first-time organizer creates and publishes their first event

Interactive steps (in-app highlights):
Step 1: Click "Create Event" → highlight the button with tooltip
Step 2: Fill event name → show progress bar (25%)
Step 3: Set date/time → calendar widget with helpful hint
Step 4: Upload image → show drag-drop area, show preview
Step 5: Set capacity → show "recommended: 50-100 for first event" hint
Step 6: Click "Publish" → confetti celebration!
Step 7: Share your event → show social share buttons

Skip option at each step. Progress saved if they leave and return.
```

---

## ✨ Writing Quality

### `@beautiful-prose`
Improve the quality and clarity of technical writing.
```
@beautiful-prose Rewrite this technical documentation to be clearer and more engaging:

Before:
"The system utilizes a microservices architecture that enables the scalable
processing of event registration requests through asynchronous message queuing
mechanisms that leverage Redis as the underlying persistence layer."

After goal:
- Active voice
- Concrete language
- Reader knows exactly what this means
- Under 25 words
```

### `@avoid-ai-writing` / `@unslop`
Remove robotic AI-generated language from documentation.
```
@avoid-ai-writing Review this documentation draft and remove AI-sounding language:
[paste draft]

Red flag phrases to remove:
- "leverage", "utilize" → use "use"
- "in the realm of" → cut entirely
- "It's worth noting that" → just say the thing
- "seamlessly", "robust", "comprehensive", "cutting-edge"
- "Furthermore", "Moreover" at sentence starts
- Passive voice: "can be done" → "you can do"
- Hedging: "may potentially" → just "can"

Rewrite: direct, human, professional technical writing.
```

### `@professional-proofreader`
Catch grammar, style, and consistency issues.
```
@professional-proofreader Proofread our API documentation for:
- Grammar and spelling errors
- Inconsistent terminology (we use both "event" and "campus event" — standardize)
- Tense inconsistency (mixing present and future)
- Ambiguous sentences (could be read two ways)
- Missing articles or awkward phrasing for ESL readers
- Code samples with inconsistent formatting

Output: list of issues with line references + suggested corrections.
```

---

## 📋 Changelog Automation

### `@changelog-automation`
Automate changelog generation from git history.
```
@changelog-automation Set up automated changelog generation:

Tool: conventional-changelog (reads Conventional Commits)
Git commit format we use:
- feat: adds new feature
- fix: bug fix
- perf: performance improvement
- docs: documentation only
- chore: maintenance
- BREAKING CHANGE: in commit body

GitHub Actions workflow:
- Trigger: git tag push (v*)
- Step: run conventional-changelog-cli
- Output: CHANGELOG.md updated
- Step: create GitHub Release with changelog section for this version
- Notify: post changelog to #engineering Slack channel

Also: show how to write good conventional commit messages that produce
a readable changelog (not just "fix: stuff").
```

---

## 🔗 Complete Technical Writer Prompt Chain

```
1️⃣  @docs-architect
    "Plan the complete documentation structure: audience, types, tools"

2️⃣  @api-documentation-generator
    "Generate full API reference with schemas, examples, error codes"

3️⃣  @openapi-spec-generation
    "Create machine-readable OpenAPI 3.0 spec for SDK generation"

4️⃣  @readme
    "Write project README: quick start, env vars, scripts"

5️⃣  @wiki-architect → @wiki-builder
    "Design and build team engineering wiki"

6️⃣  @architecture-decision-records
    "Document key technical decisions made during development"

7️⃣  @technical-tutorials
    "Write getting started tutorial for API consumers"

8️⃣  @beautiful-prose → @avoid-ai-writing
    "Polish all writing: clarity, directness, remove AI language"

9️⃣  @changelog-automation
    "Set up automated changelog from Conventional Commits"
```

---

## 💡 Pro Tips for Technical Writers

1. **`@docs-architect` before writing a single page** — structure prevents documentation debt
2. **`@openapi-spec-generation` from code, not description** — single source of truth
3. **`@avoid-ai-writing` after any AI-assisted drafting** — documentation needs to sound human
4. **`@changelog-automation` from day one** — retroactively writing changelogs is a nightmare
5. **`@tutorial-engineer` vs `@technical-tutorials`** — engineer = interactive/in-app, tutorials = standalone guide
6. **`@architecture-decision-records`** — write ADRs in the moment, future developers will thank you
