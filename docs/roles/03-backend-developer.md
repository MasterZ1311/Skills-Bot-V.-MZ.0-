# ⚙️ Backend Developer — Skills Guide

Backend developers build the engine of the application — APIs, services, data processing, queues, and business logic. This guide covers language expertise, framework patterns, API design, architecture, async processing, and security.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Languages | `@nodejs-best-practices`, `@python-pro`, `@golang-pro`, `@java-pro`, `@rust-pro` |
| Frameworks | `@fastapi-pro`, `@nestjs-expert`, `@django-pro`, `@laravel-expert`, `@hono` |
| API Design | `@api-design-principles`, `@graphql-architect`, `@openapi-spec-generation` |
| Architecture | `@backend-architect`, `@microservices-patterns`, `@event-sourcing-architect` |
| Async & Queues | `@bullmq-specialist`, `@inngest`, `@trigger-dev`, `@temporal-golang-pro` |
| Auth & Security | `@auth-implementation-patterns`, `@backend-security-coder`, `@secrets-management` |
| Code Patterns | `@clean-code`, `@error-handling-patterns`, `@fp-backend`, `@async-python-patterns` |
| Documentation | `@api-documentation-generator`, `@openapi-spec-generation` |

---

## 🌐 Language Expert Skills

### `@nodejs-best-practices`
Node.js patterns, async/await, streams, clustering, performance.
```
@nodejs-best-practices Review our Node.js API server:
- Are we handling async errors correctly?
- Do we have proper graceful shutdown?
- Memory leak risks?
- Event loop blocking operations?
- Rate limiting and connection pooling?
```

### `@python-pro` / `@async-python-patterns`
Python backend development, async patterns with asyncio and FastAPI.
```
@async-python-patterns Convert our synchronous event notification system to async:
- Use asyncio for concurrent email sending
- aiohttp for external API calls
- Rate limiting with asyncio.Semaphore
- Error handling that doesn't block the event loop
```

### `@golang-pro` / `@go-concurrency-patterns`
Go backend patterns, goroutines, channels, concurrency primitives.
```
@go-concurrency-patterns Design a concurrent job processor in Go:
- Worker pool with configurable size
- Job queue via channels
- Graceful shutdown on SIGTERM
- Dead letter queue for failed jobs
- Prometheus metrics for queue depth
```

### `@java-pro` / `@kotlin-coroutines-expert`
Java and Kotlin backend with Spring Boot, coroutines, reactive streams.
```
@java-pro Implement a Spring Boot REST endpoint for event registration:
- @RestController with proper exception handling
- @Service layer with transaction management
- Repository pattern with JPA
- Bean validation on request DTO
- Custom exception handler returning RFC 7807 problem details
```

### `@rust-pro` / `@rust-async-patterns`
Rust backend with Actix-web or Axum, async Tokio runtime.
```
@rust-async-patterns Build an Axum handler for event registration:
- Extract typed request body with serde
- Database query with SQLx
- Async error propagation with thiserror/anyhow
- Return typed JSON response
```

### `@csharp-pro` / `@dotnet-backend` / `@dotnet-backend-patterns`
.NET backend with ASP.NET Core.
```
@dotnet-backend-patterns Implement a CQRS pattern in ASP.NET Core:
- MediatR for command/query handling
- Command handler for registration
- Query handler for event listing
- FluentValidation on commands
- Problem details error responses
```

### Other language skills:
- `@php-pro` — PHP / Laravel
- `@ruby-pro` — Ruby / Rails
- `@elixir-pro` — Elixir / Phoenix
- `@scala-pro` — Scala / Akka
- `@haskell-pro` — Haskell functional backend

---

## 🛠️ Framework Skills

### `@fastapi-pro` / `@fastapi-templates` / `@fastapi-router-py`
FastAPI with async, Pydantic validation, dependency injection.
```
@fastapi-pro Build the registration router for our events API:
- POST /events/{event_id}/register (authenticated)
- GET /events/{event_id}/registrations (admin only)
- DELETE /registrations/{id} (cancel)
Include: Pydantic models, dependency injection for DB session,
HTTPException error handling, OAuth2 JWT authentication.
```

### `@nestjs-expert`
NestJS modules, providers, guards, interceptors, pipes.
```
@nestjs-expert Create a complete NestJS module for event notifications:
- NotificationModule with providers
- NotificationService using Bull queue
- NotificationController for webhook intake
- Guards: JwtAuthGuard, RolesGuard
- DTO with class-validator
- Module exports for other modules to use
```

### `@django-pro`
Django REST framework, ORM, class-based views.
```
@django-pro Implement event registration in Django REST framework:
- ModelSerializer for Registration
- Custom permission class (IsStudentOrReadOnly)
- ViewSet with action decorators
- Custom queryset filtering
- Atomic transaction for capacity management
```

### `@laravel-expert`
Laravel controllers, Eloquent, jobs, queues.
```
@laravel-expert Build the registration system in Laravel:
- Registration Controller with Form Request validation
- Eloquent model with relationships and scopes
- Job: SendRegistrationConfirmation (queued)
- Policy for authorization
- Resource for API response transformation
```

### `@hono`
Hono.js for edge-native, ultra-fast APIs.
```
@hono Build an event listing API endpoint with Hono:
- Route handler with typed context
- Zod input validation middleware
- Database query via Drizzle
- Response caching headers
- Deploy to Cloudflare Workers
```

---

## 📐 API Design

### `@api-design-principles`
Before implementing — get the API shape right.
```
@api-design-principles Review our events API design:
Propose corrections for:
- Resource naming consistency
- HTTP verb correctness
- Error response format (RFC 7807)
- Versioning strategy (URL vs header)
- Pagination approach (cursor vs offset)
- Idempotency for registration endpoints
```

### `@graphql-architect` / `@graphql` / `@graphql-schema`
GraphQL schema design, resolvers, subscriptions.
```
@graphql-architect Design the GraphQL schema for our events platform:
- Types: Event, Registration, User, Ticket
- Queries: events(filter, pagination), event(id), myRegistrations
- Mutations: registerForEvent, cancelRegistration
- Subscriptions: eventCapacityUpdate(eventId)
- Directives: @auth, @deprecated
```

### `@openapi-spec-generation` / `@api-documentation-generator`
Generate OpenAPI 3.0 specs from code or description.
```
@openapi-spec-generation Generate a complete OpenAPI 3.0 spec for:
POST /api/v1/events/{eventId}/register
- Auth: Bearer JWT
- Request body: { ticketCount, paymentMethodId }
- Responses: 201 (created), 400 (validation), 409 (sold out), 422 (payment failed)
Include: examples, security scheme, component schemas.
```

### `@grpc-golang`
gRPC service design and implementation in Go.
```
@grpc-golang Design the gRPC service definition for internal microservices:
- EventService: GetEvent, ListEvents, CreateEvent
- RegistrationService: CreateRegistration, CancelRegistration, GetTicket
Include: proto3 syntax, stream where appropriate, error codes.
```

---

## 🏗️ Architecture Patterns

### `@microservices-patterns`
Breaking a monolith into services, service contracts, inter-service communication.
```
@microservices-patterns Our events monolith needs to be split.
Propose microservice boundaries for: Users, Events, Registrations, Payments, Notifications.
For each: responsibilities, API contract, data ownership, communication style (sync REST vs async queue).
```

### `@event-sourcing-architect` / `@event-store-design`
Event sourcing and CQRS for audit trails and event-driven systems.
```
@event-sourcing-architect Design event sourcing for our registration system:
- Events: RegistrationCreated, RegistrationCancelled, PaymentProcessed
- Aggregate: Registration
- Event store schema (PostgreSQL)
- Projection: current registration state
- Snapshot strategy for performance
```

### `@cqrs-implementation`
Command Query Responsibility Segregation.
```
@cqrs-implementation Implement CQRS for the events read model:
- Command: CreateEvent, UpdateEventCapacity
- Query: ListEventsWithRegistrationCount
- Read model: separate optimized table
- Eventual consistency handling
```

### `@saga-orchestration`
Distributed transaction patterns with sagas.
```
@saga-orchestration Design a saga for event registration that spans:
1. ReserveCapacity (Events service)
2. ProcessPayment (Payment service)
3. CreateTicket (Ticket service)
4. SendConfirmation (Notification service)
Handle rollback for each step. Choreography or orchestration? Recommend and implement.
```

### `@ddd-strategic-design` / `@ddd-tactical-patterns`
Domain modeling with aggregates, entities, value objects.
```
@ddd-tactical-patterns Model the Event aggregate:
- Aggregate root: Event
- Value objects: EventCapacity, TicketPrice, EventSchedule
- Domain events: EventPublished, EventCancelled, CapacityChanged
- Business rules enforced in aggregate methods
- Repository interface
```

---

## ⚡ Async Processing & Queues

### `@bullmq-specialist`
BullMQ (Node.js Redis queue) for background jobs.
```
@bullmq-specialist Set up BullMQ for our notifications:
- NotificationQueue with retry strategy
- Worker: sends email, falls back to SMS
- Dead letter queue after 3 failures
- Dashboard with Bull Board
- Rate limiting: 100 emails/minute
```

### `@inngest` / `@trigger-dev`
Managed background jobs with durable execution.
```
@inngest Create Inngest functions for:
1. sendRegistrationConfirmation: triggered on Registration.Created event
2. processCapacityRelease: triggered on Registration.Cancelled
3. generateDailyReport: scheduled daily at 8am IST
Include retry configuration and error handling.
```

### `@temporal-golang-pro` / `@temporal-python-pro`
Temporal for complex, durable workflows.
```
@temporal-golang-pro Design a Temporal workflow for event registration:
Activities: ValidateCapacity, ProcessPayment, CreateTicket, SendEmail
- Max attempts: 3 for payment, 5 for email
- Compensation workflow on payment failure
- Visibility: query current state from API
```

---

## 🔐 Security

### `@auth-implementation-patterns`
Designing the auth strategy.
```
@auth-implementation-patterns Design JWT auth for our events API:
- Access token: 15min expiry
- Refresh token: 7 days, stored in httpOnly cookie
- Token rotation on refresh
- Revocation strategy (Redis blocklist)
- API key auth for admin webhook endpoints
```

### `@backend-security-coder`
Security-focused code review for backend.
```
@backend-security-coder Review the registration endpoint for:
- SQL injection (parameterized queries?)
- Mass assignment vulnerabilities
- Rate limiting gaps
- Authentication bypass possibilities
- Sensitive data in logs
```

### `@secrets-management`
Secrets, environment variables, and key rotation.
```
@secrets-management Set up secrets management for our Node.js API:
- Local: .env with dotenv-vault
- Staging/Prod: AWS Secrets Manager / Azure Key Vault
- Database credentials rotation (no downtime)
- CI/CD: GitHub Actions secrets
- Audit logging for secret access
```

---

## 🔗 Complete Backend Prompt Chain

```
1️⃣  @backend-architect
    "Design service layer: RegistrationService with atomic patterns"

2️⃣  @api-design-principles
    "Define REST contract: verbs, responses, error codes, versioning"

3️⃣  @nestjs-expert (or @fastapi-pro)
    "Implement the endpoint: guards, DTOs, service calls"

4️⃣  @auth-implementation-patterns
    "Add JWT middleware and role-based access"

5️⃣  @error-handling-patterns
    "Consistent error handling: validation vs business vs system errors"

6️⃣  @bullmq-specialist (or @inngest)
    "Queue async work: email, capacity release, reporting"

7️⃣  @backend-security-coder
    "Security review: injection, auth bypass, data exposure"

8️⃣  @openapi-spec-generation
    "Generate OpenAPI spec for frontend and QA teams"

9️⃣  @jest-skill (or @pytest-skill)
    "Unit tests: service logic, validation, error cases"

🔟  @k6-load-testing
    "Load test: 500 concurrent registrations, measure p99 latency"
```
