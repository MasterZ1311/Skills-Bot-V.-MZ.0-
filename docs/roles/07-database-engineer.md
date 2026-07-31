# 🗄️ Database Engineer — Skills Guide

Database engineers design schemas, write queries, manage migrations, ensure performance, and maintain data reliability. This guide covers SQL, NoSQL, ORMs, cloud databases, vector databases, and data warehousing.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| SQL | `@sql-pro`, `@postgresql`, `@postgresql-optimization`, `@database-admin` |
| ORMs | `@prisma-expert`, `@drizzle-orm-expert`, `@zod-validation-expert` |
| Cloud DBs | `@supabase`, `@neon-postgres`, `@azure-cosmos-py`, `@claimable-postgres` |
| Design | `@database-design`, `@database-architect`, `@domain-modeling` |
| Migration | `@database-migration`, `@database-migrations-sql-migrations` |
| Vector DBs | `@vector-database-engineer`, `@weaviate`, `@hybrid-search-implementation` |
| Analytics | `@snowflake-development`, `@dbt-transformation-patterns`, `@data-quality-frameworks` |
| Monitoring | `@monte-carlo-*`, `@database-optimizer` |

---

## 🐘 PostgreSQL & SQL

### `@sql-pro`
Advanced SQL: window functions, CTEs, subqueries, complex joins.
```
@sql-pro Write a complex query for our events dashboard:
- Events with total registrations, revenue, capacity utilization %
- Top 10 events by revenue this month
- Waitlist count per event
- Registration trend over last 30 days (daily aggregation)
Use CTEs for readability. Explain the execution plan.
```

### `@postgresql`
PostgreSQL-specific features: JSONB, arrays, full-text search, triggers, extensions.
```
@postgresql Add full-text search to our events:
- tsvector column on events table (title + description + tags)
- GIN index for performance
- Query with ts_rank for relevance scoring
- Highlight matching text in results
- Support for partial word matching
Use PostgreSQL's native FTS, not Elasticsearch.
```

### `@postgresql-optimization` / `@sql-optimization-patterns`
Query optimization, index design, EXPLAIN ANALYZE.
```
@postgresql-optimization This query takes 3 seconds on our events table (500k rows):
SELECT e.*, COUNT(r.id) as reg_count 
FROM events e 
LEFT JOIN registrations r ON e.id = r.event_id 
WHERE e.status = 'published' AND e.date > NOW()
GROUP BY e.id 
ORDER BY e.date;

Run EXPLAIN ANALYZE, identify bottleneck, add indexes, rewrite if needed.
```

### `@database-admin`
DBA tasks: backups, replication, user management, vacuum, pg_stat.
```
@database-admin Set up PostgreSQL for production:
- Connection pooling with PgBouncer (transaction mode)
- Streaming replication to read replica
- Automated backups: pg_dump daily + WAL archiving
- Vacuum tuning: autovacuum settings for high-write tables
- Role-based access: app_user (CRUD), reporting_user (SELECT), admin
```

### `@database-optimizer`
Holistic database performance optimization.
```
@database-optimizer Diagnose our slow database performance:
Our API has p99 latency of 800ms, mostly on DB queries.
Analyze: missing indexes, N+1 queries, missing connection pooling,
table bloat, missing vacuuming, configuration tuning (work_mem, shared_buffers).
```

---

## 🔧 ORMs & Schema Tools

### `@prisma-expert`
Prisma schema design, migrations, queries, type safety.
```
@prisma-expert Design the complete Prisma schema for our events platform:

model User {
  - id, email, name, role (STUDENT/ADMIN)
  - Relations to: registrations, events (as organizer)
}
model Event {
  - id, title, description, date, capacity, status
  - JSONB metadata field for flexible attributes
  - Relations to: registrations, organizer
}
model Registration {
  - id, status (PENDING/CONFIRMED/CANCELLED)
  - Composite unique: (userId, eventId)
}

Include: indexes, cascades, soft deletes where appropriate.
```

```
@prisma-expert Write Prisma queries for:
1. Paginated events with registration count (avoid N+1)
2. Atomic registration: create + decrement capacity in one transaction
3. Full-text search across title and description
4. Admin dashboard: events grouped by month with revenue totals
```

### `@drizzle-orm-expert` / `@drizzle-migration-conflict`
Drizzle ORM with TypeScript, edge-compatible.
```
@drizzle-orm-expert Set up Drizzle ORM for our Neon PostgreSQL:
- Schema definition in TypeScript
- Relations: events → registrations → users
- Type-safe query builder usage
- Migration workflow with drizzle-kit
- Edge-compatible for Cloudflare Workers
```

```
@drizzle-migration-conflict We have a migration conflict after merging two branches.
Both added columns to the events table.
[paste conflicting migration files]
How to resolve without data loss?
```

### `@zod-validation-expert`
Zod schemas for runtime validation aligned with DB schema.
```
@zod-validation-expert Create Zod schemas that match our Prisma models:
- EventCreateSchema with all required validations
- RegistrationCreateSchema
- Infer TypeScript types from schemas
- Shared between frontend and backend
- Custom validators: date must be in future, capacity > 0
```

---

## ☁️ Cloud Databases

### `@supabase` / `@supabase-postgres-best-practices`
Supabase with RLS, Edge Functions, Realtime.
```
@supabase Set up Supabase for our events platform:
- Schema via Supabase migrations (SQL)
- Row Level Security: students see own data, admins see all
- Realtime subscription for live capacity updates
- Storage bucket for event images with signed URLs
- Edge Function for post-registration webhook
```

### `@neon-postgres` / `@neon-postgres-branches` / `@using-neon`
Neon serverless PostgreSQL with branching.
```
@neon-postgres-branches Use Neon branching in our development workflow:
- Main branch: production database
- Staging branch: mirrors production
- Create branch per PR (automated in CI)
- Run migrations on branch, verify, then promote
- Delete branch after PR merge
Show: CLI commands and GitHub Actions integration.
```

### `@neon-postgres-egress-optimizer`
Minimize Neon data transfer costs.
```
@neon-postgres-egress-optimizer Our Neon bill has high egress costs.
Audit our query patterns:
- Are we fetching columns we don't use?
- Can we aggregate on DB instead of application?
- Connection pooling to reduce connection overhead?
- Caching frequently-read, rarely-updated data in Redis?
```

---

## 🔀 Database Migrations

### `@database-migration` / `@database-migrations-sql-migrations`
Safe, reversible, zero-downtime migrations.
```
@database-migrations-sql-migrations We need to add payment tracking to events:
New tables: payments (id, registration_id, amount, status, stripe_payment_id)
New column: registrations.payment_id (nullable FK)

Write migration that:
1. Creates payments table
2. Adds nullable FK column (no downtime)
3. Backfills from existing payment data
4. Makes column NOT NULL in a follow-up migration
5. Is fully reversible (down migration)
```

### `@database-migrations-migration-observability`
Monitor migrations as they run.
```
@database-migrations-migration-observability Add observability to our migrations:
- Log migration start/end with duration
- Alert if migration takes > 5 minutes (long lock risk)
- Track schema version in application health endpoint
- Rollback trigger if error rate spikes post-migration
```

---

## 🔍 Vector Databases

### `@vector-database-engineer` / `@weaviate` / `@weaviate-cookbooks`
Vector DBs for semantic search and AI applications.
```
@vector-database-engineer Design semantic search for our events:
- Embed event title + description using OpenAI/sentence-transformers
- Store in Weaviate (or pgvector)
- Query: "outdoor music events near campus" → find semantically similar events
- Hybrid search: combine keyword + semantic
- Reranking with cross-encoder model
```

### `@hybrid-search-implementation`
Combine keyword and vector search.
```
@hybrid-search-implementation Implement hybrid search for our events platform:
- Keyword search: PostgreSQL full-text search (tsvector)
- Semantic search: pgvector with OpenAI embeddings
- Fusion: Reciprocal Rank Fusion (RRF)
- Weights: 70% semantic, 30% keyword
- Filter by: date, category, location before search
```

### `@vector-index-tuning`
HNSW and IVFFlat index tuning for performance.

---

## 📊 Analytics & Warehousing

### `@snowflake-development`
Snowflake SQL, stages, streams, tasks, data sharing.
```
@snowflake-development Set up Snowflake for events analytics:
- Stage: load daily registration exports from S3
- Table: events_fact with date, event, revenue, count dimensions
- Stream + Task: incremental processing
- View: executive_dashboard (materialized daily)
- Share: read-only share for reporting tool
```

### `@dbt-transformation-patterns`
dbt models, tests, documentation for transformations.
```
@dbt-transformation-patterns Create dbt models for our events data:
- Staging: stg_registrations (clean raw data)
- Intermediate: int_registrations_enriched (join with events, users)
- Mart: mart_event_performance (aggregated KPIs per event)
- Tests: not_null, unique, referential integrity
- Documentation with column descriptions
```

### `@data-quality-frameworks`
Data quality checks, monitoring, and alerting.
```
@data-quality-frameworks Implement data quality monitoring:
- Completeness: registration emails not null
- Freshness: events data updated < 1hr ago
- Accuracy: capacity >= confirmed_registrations
- Consistency: payment_status matches registration_status
Alert on any failing checks.
```

---

## 🔍 Monte Carlo — Data Observability

The Monte Carlo suite (`@monte-carlo-*`) provides 9 specialized skills for data quality and observability:

| Skill | When to Use |
|---|---|
| `@monte-carlo-monitor-creation` | Set up automated data monitors |
| `@monte-carlo-analyze-root-cause` | Investigate data quality incidents |
| `@monte-carlo-asset-health` | Check overall data asset health |
| `@monte-carlo-monitoring-advisor` | Get recommendations for what to monitor |
| `@monte-carlo-performance-diagnosis` | Diagnose slow pipelines or queries |
| `@monte-carlo-prevent` | Proactive data quality prevention |
| `@monte-carlo-remediation` | Fix detected data quality issues |
| `@monte-carlo-storage-cost-analysis` | Analyze and optimize storage costs |
| `@monte-carlo-push-ingestion` | Set up data ingestion monitoring |

```
@monte-carlo-monitoring-advisor Our events database feeds an analytics dashboard.
What should we monitor to catch data quality issues early?
Tables: events, registrations, payments
Key metrics: registration count, revenue, capacity utilization
```

---

## 🔗 Complete Database Prompt Chain

```
1️⃣  @database-architect
    "Design overall data model: entities, relationships, access patterns"

2️⃣  @database-design
    "Write detailed schema with indexes, constraints, naming conventions"

3️⃣  @prisma-expert (or @drizzle-orm-expert)
    "Translate to ORM schema, set up migrations"

4️⃣  @database-migrations-sql-migrations
    "Write safe, reversible migrations with rollback plans"

5️⃣  @postgresql-optimization
    "Index design, query analysis, connection pooling setup"

6️⃣  @neon-postgres (or @supabase)
    "Configure cloud database: branching, RLS, connection pooling"

7️⃣  @vector-database-engineer
    "Add pgvector for semantic search capability"

8️⃣  @monte-carlo-monitor-creation
    "Set up data quality monitors and alerts"

9️⃣  @database-admin
    "Production hardening: backups, replication, security, vacuum tuning"
```
