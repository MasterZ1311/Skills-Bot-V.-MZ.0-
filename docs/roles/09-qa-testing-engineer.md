# ✅ QA & Testing Engineer — Skills Guide

QA engineers ensure software quality through automated and manual testing, TDD practices, performance testing, and accessibility auditing. This guide covers the full testing spectrum from unit to E2E, plus debugging and code review.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| Unit Testing | `@jest-skill`, `@vitest-skill`, `@pytest-skill`, `@junit-5-skill` |
| E2E Testing | `@playwright-skill`, `@cypress-skill`, `@selenium-skill`, `@puppeteer-skill` |
| TDD | `@tdd`, `@tdd-orchestrator`, `@tdd-workflow`, `@tdd-workflows-tdd-cycle` |
| Performance | `@k6-load-testing`, `@performance-testing-review-ai-review` |
| Mobile Testing | `@appium-skill`, `@android-ui-journey-testing`, `@awt-e2e-testing` |
| API Testing | `@postman-collection-generator`, `@postman-newman-automation` |
| Accessibility | `@wcag-audit-patterns`, `@accesslint-audit`, `@screen-reader-testing` |
| Debugging | `@systematic-debugging`, `@debugging-toolkit`, `@phase-gated-debugging` |
| Code Review | `@code-review-excellence`, `@differential-review`, `@comprehensive-review-full-review` |

---

## 🧪 Unit & Integration Testing

### `@jest-skill`
Jest configuration, mocking, coverage, snapshots.
```
@jest-skill Set up Jest for our Node.js API project:
- jest.config.ts with TypeScript support
- Module path aliases
- Mock strategy: jest.mock() for Prisma, Stripe, email
- Coverage threshold: 80% branches
- Test database: in-memory SQLite for integration tests
Write example tests for RegistrationService:
- Happy path: successful registration
- Error: event at capacity
- Error: duplicate registration
- Atomic transaction: verify rollback on payment failure
```

### `@vitest-skill`
Vitest for Vite-based projects (faster, ESM-native).
```
@vitest-skill Convert our Jest test suite to Vitest:
- Install and configure vitest.config.ts
- Update imports (vitest vs jest)
- Migrate mocks (vi.mock instead of jest.mock)
- Browser mode for DOM testing
- Coverage with v8 provider
Show before/after for 3 representative test files.
```

### `@pytest-skill`
Python testing with pytest, fixtures, parametrize.
```
@pytest-skill Write a comprehensive test suite for our FastAPI registration endpoint:
- Fixtures: test client, mock database, test user factory
- Parametrize: test multiple invalid input combinations
- Async tests with anyio
- Mock: Stripe SDK, email service
- Coverage: 95% on registration module
```

### `@junit-5-skill` / `@testng-skill`
Java testing frameworks.
```
@junit-5-skill Write JUnit 5 tests for our Spring Boot RegistrationService:
- @ExtendWith(MockitoExtension) for mocking
- @SpringBootTest for integration tests
- @Sql for database setup/teardown
- @ParameterizedTest for edge cases
- Test: capacity check, atomic transaction, email dispatch
```

---

## 🎭 End-to-End Testing

### `@playwright-skill`
Playwright setup, page objects, fixtures, visual testing.
```
@playwright-skill Set up Playwright for our events platform:
1. Install and configure playwright.config.ts
2. Page Object Model: EventsPage, RegistrationModal, TicketsPage
3. Fixtures: authenticated student, authenticated admin
4. Tests:
   - Student registers for event successfully
   - Student sees sold-out badge when event full
   - Admin sees registration in dashboard
5. Visual regression: screenshot comparison for EventCard
6. CI: run on PR with --reporter=github
```

```
@playwright-skill Write a test for the complete payment flow:
- Log in as student (fixture)
- Navigate to events listing
- Filter by category "Music"
- Click first available event
- Click "Register Now"
- Fill registration form
- Use Stripe test card (4242 4242 4242 4242)
- Assert: success message, redirect to /tickets/:id
- Assert: QR code visible on ticket page
- Assert: confirmation email sent (check mock)
```

### `@cypress-skill`
Cypress for web app testing.
```
@cypress-skill Set up Cypress for our Next.js app:
- cypress.config.ts with baseUrl and custom commands
- Custom command: cy.login(email, password) using API shortcut
- Intercept: stub external API calls (Stripe, email)
- Component testing for EventCard
- E2E: registration flow, admin dashboard
- GitHub Actions: Cypress Cloud parallel execution
```

### `@puppeteer-skill`
Puppeteer for web scraping and browser automation tests.
```
@puppeteer-skill Use Puppeteer to test our PDF ticket generation:
- Launch browser headless
- Authenticate and register for event
- Navigate to /tickets/:id
- Trigger PDF download
- Assert PDF content (title, QR code present)
- Also: screenshot each ticket page for visual record
```

---

## 🔴 TDD (Test-Driven Development)

### `@tdd` / `@tdd-orchestrator`
TDD philosophy and orchestration for a team.
```
@tdd-orchestrator Plan TDD implementation for our team working on waitlist feature.
How to structure:
- Red: what tests to write first?
- Green: minimum code to pass?
- Refactor: what to clean up?
Provide the full TDD cycle for WaitlistService.addToWaitlist(userId, eventId).
```

### `@tdd-workflows-tdd-red`
Writing failing tests first.
```
@tdd-workflows-tdd-red Write failing tests for WaitlistService.addToWaitlist:
Requirements:
- Cannot join waitlist if event has open spots
- Cannot join waitlist twice
- Waitlist has max limit (200 per event)
- Returns position in waitlist

Write the failing test cases BEFORE writing any implementation.
```

### `@tdd-workflows-tdd-green`
Making failing tests pass with minimal code.
```
@tdd-workflows-tdd-green Here are my failing tests:
[paste tests from previous step]
Write the MINIMUM implementation of WaitlistService.addToWaitlist
to make all tests pass. No over-engineering — just enough to pass.
```

### `@tdd-workflows-tdd-refactor`
Refactoring after tests pass.
```
@tdd-workflows-tdd-refactor Tests are passing. Now refactor WaitlistService:
[paste current implementation]
Improve: extract constants, cleaner error messages, remove duplication,
better method names. All tests must still pass.
```

---

## ⚡ Performance Testing

### `@k6-load-testing`
k6 load testing scripts and analysis.
```
@k6-load-testing Write k6 load tests for our events API:
1. Baseline: 10 virtual users, 1 minute
   - GET /api/events (list)
   - GET /api/events/:id (detail)
   
2. Load: 100 VUs, 5 minutes
   - Mixed: 70% reads, 20% registration, 10% ticket fetch
   
3. Stress test: ramp to 500 VUs
   - At what point does p99 latency exceed 2s?
   
4. Spike: instant jump to 300 VUs
   - Simulates event announcement going viral

Thresholds: http_req_duration p(95) < 500ms, error_rate < 1%
```

---

## 📱 Mobile Testing

### `@appium-skill`
Appium for cross-platform mobile testing.
```
@appium-skill Set up Appium tests for our events mobile app:
- Appium server config for iOS simulator + Android emulator
- Test: login, browse events, register
- Page Object pattern for mobile screens
- Run in GitHub Actions with simulator
```

### `@android-ui-journey-testing`
Android-native UI tests with Espresso / Compose Testing.
```
@android-ui-journey-testing Write Android UI journey tests for:
1. Launch app → see events list
2. Tap event → see detail screen with register button
3. Tap register → form fills and submits
4. See success screen with ticket
Using Compose UI Testing API.
```

---

## 🌐 API Testing

### `@postman-collection-generator`
Generate Postman collections from specs or descriptions.
```
@postman-collection-generator Create a Postman collection for our events API:
- Environment: dev, staging, prod (with variable URLs)
- Auth: Bearer token stored as variable, auto-refreshed
- Folders: Events, Registration, Tickets, Admin
- Tests in each request: status code, response schema, data assertions
- Pre-request: set auth token if expired
Export as Postman v2.1 JSON.
```

### `@postman-newman-automation`
Run Postman collections in CI with Newman.
```
@postman-newman-automation Run our Postman collection in CI:
- Newman CLI command with environment file
- GitHub Actions step after deployment to staging
- JUnit reporter for test results in CI UI
- Fail CI if any test fails
- Slack notification with test summary
```

---

## ♿ Accessibility Testing

### `@wcag-audit-patterns`
WCAG 2.1 / 2.2 audit patterns.
```
@wcag-audit-patterns Create an accessibility test plan for our events platform:
Level AA compliance required.
- Automated: axe-core in Playwright, every page
- Manual: keyboard navigation test script
- Screen reader: NVDA + Chrome test scenarios
- Color contrast: all text/background combinations
- Focus management: modal open/close
- Dynamic content: ARIA live regions for capacity updates
```

### `@accesslint-audit` / `@accesslint-diff`
CI accessibility checks on PRs.
```
@accesslint-diff Set up accessibility diff checking on PRs:
- Run axe-core on changed pages
- Comment on PR with new accessibility violations introduced
- Block merge if Critical violations added
- Allow temporary bypass with justification comment
```

### `@screen-reader-testing`
Testing with actual screen readers.
```
@screen-reader-testing Create screen reader test scripts for our events platform:
Using NVDA + Chrome.
Test scenarios:
1. Navigate to events page with keyboard only
2. Find and read event card information
3. Open registration modal using keyboard
4. Complete registration form with screen reader guidance
5. Hear success announcement after registration
What ARIA patterns to use for each scenario?
```

---

## 🐛 Debugging

### `@systematic-debugging`
Structured debugging methodology.
```
@systematic-debugging Our registration endpoint intermittently fails with 500.
It happens ~1% of requests and only in production.
Error: "Cannot read properties of undefined (reading 'id')"
Stack trace: [paste]

Guide me through systematic debugging:
- Hypothesis formation
- What to log/instrument
- How to reproduce locally
- Likely root causes given the error
```

### `@debugging-toolkit` / `@phase-gated-debugging`
Tools and phases for systematic debugging.
```
@phase-gated-debugging Use phase-gated debugging for this bug:
Bug: Students can register twice for the same event despite unique constraint
Phase 1: Reproduce
Phase 2: Localize (frontend? API? DB?)
Phase 3: Root cause (race condition? missing check?)
Phase 4: Fix
Phase 5: Prevent recurrence (test, monitoring)
```

---

## 🔍 Code Review

### `@code-review-excellence`
Thorough, constructive code review.
```
@code-review-excellence Review this PR (waitlist feature):
[paste diff]
Check:
- Correctness: does it handle all edge cases?
- Security: any injection or auth bypass?
- Performance: N+1 queries, unnecessary DB calls?
- Test coverage: what's missing?
Format as: [BLOCKING] must fix, [SUGGESTION] optional improvement, [PRAISE] good patterns.
```

### `@differential-review`
Focused review on what changed between two versions.
```
@differential-review Compare these two implementations of the capacity check:
Version A: [paste]
Version B: [paste]
Which is more correct, more performant, more readable?
What are the edge cases each handles or misses?
```

---

## 🔗 Complete QA Prompt Chain

```
1️⃣  @tdd-workflows-tdd-red
    "Write failing tests for new feature (waitlist)"

2️⃣  @tdd-workflows-tdd-green
    "Implement minimum code to pass tests"

3️⃣  @tdd-workflows-tdd-refactor
    "Refactor while keeping tests green"

4️⃣  @jest-skill (or @vitest-skill / @pytest-skill)
    "Add edge case unit tests, mock external dependencies"

5️⃣  @playwright-skill
    "Write E2E tests for waitlist: join, position display, notification"

6️⃣  @k6-load-testing
    "Load test: 100 concurrent users joining waitlist simultaneously"

7️⃣  @accesslint-audit
    "Accessibility check on new waitlist UI"

8️⃣  @postman-collection-generator
    "Add waitlist endpoints to API test collection"

9️⃣  @code-review-excellence
    "Review PR before merge: correctness, security, performance"
```
