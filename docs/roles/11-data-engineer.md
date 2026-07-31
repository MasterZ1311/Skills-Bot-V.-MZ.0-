# 📊 Data Engineer — Skills Guide

Data engineers build pipelines, transform raw data into analytics-ready tables, ensure data quality, and power BI dashboards and ML systems.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Pipelines | `@data-engineering-data-pipeline`, `@airflow-dag-patterns`, `@data-engineering-data-driven-feature` |
| Transformation | `@dbt-transformation-patterns`, `@spark-optimization`, `@polars` |
| Warehousing | `@snowflake-development`, `@dbt-transformation-patterns` |
| Quality | `@data-quality-frameworks`, `@monte-carlo-monitor-creation`, `@monte-carlo-analyze-root-cause` |
| Vector / Search | `@vector-index-tuning`, `@algolia-search`, `@similarity-search-patterns` |
| Analytics | `@data-scientist`, `@matplotlib`, `@plotly`, `@seaborn`, `@statsmodels`, `@scikit-learn` |
| Streaming | `@azure-eventhub-py`, `@aws-serverless-eda` |
| Observability | All 9 `@monte-carlo-*` skills |

---

## 🔁 Pipeline Design

### `@data-engineering-data-pipeline`
Design and build end-to-end data pipelines.
```
@data-engineering-data-pipeline Design a data pipeline for our events analytics platform:
Source: PostgreSQL (transactional — events, registrations, payments)
Destination: Snowflake (analytics warehouse)

Define:
- Extraction: CDC with Debezium OR daily pg_dump → Parquet → S3
- Transformation: clean, normalize, compute derived columns
- Load: upsert into Snowflake dimension and fact tables
- Schedule: hourly for registrations, daily for events summary
- Error handling: dead letter queue, Slack alert on failure
- Idempotency: safe to re-run without duplicates
```

### `@airflow-dag-patterns`
Orchestrate pipelines with Apache Airflow DAGs.
```
@airflow-dag-patterns Create Airflow DAGs for our events data platform:

DAG 1: daily_events_sync (schedule: 0 2 * * *)
  Tasks: extract_events → validate_schema → transform_events → load_snowflake → notify_success
  Retry: 3 retries, 5-minute wait
  SLA: 4am deadline, email on miss

DAG 2: hourly_registration_sync (schedule: 0 * * * *)
  Tasks: extract_new_registrations → enrich_with_user → load_registrations → update_kpi_cache
  Upstream: depends on users dimension being fresh

Include: XCom for passing row counts, Slack callback on failure, task-level logging.
```

### `@data-engineering-data-driven-feature`
Build data-driven product features (recommendation, personalization, scoring).
```
@data-engineering-data-driven-feature Build a "Recommended Events" data pipeline:
Goal: score all (user, event) pairs for personalized recommendations

Feature engineering:
- User features: categories attended, days active, registration rate
- Event features: category, capacity fill rate, avg rating, organizer reputation
- Interaction: user attended similar past events?

Pipeline:
- Daily batch compute in Spark/dbt
- Store top-10 recommendations per user in Redis
- Expire on event date or when event fills
```

---

## 🔄 Transformation

### `@dbt-transformation-patterns`
Build clean, tested, documented dbt models.
```
@dbt-transformation-patterns Build a dbt project for our events analytics:

Staging layer (stg_*):
- stg_events: cast types, rename columns, filter deleted records
- stg_registrations: standardize status enum, add is_confirmed boolean
- stg_payments: clean amounts, add payment_method label

Intermediate layer (int_*):
- int_registrations_enriched: join registrations + events + users

Mart layer (mart_*):
- mart_event_performance: per-event KPIs (revenue, occupancy %, attendee count)
- mart_daily_summary: aggregated daily snapshot for dashboard

Tests: not_null, unique on primary keys, accepted_values for status,
relationships (foreign key) checks.
Documentation: description + column comments for every model.
```

### `@spark-optimization`
Optimize Apache Spark jobs for large-scale processing.
```
@spark-optimization Our Spark job processing 50M registration events takes 45 minutes.
Target: under 10 minutes.

Review for:
- Partition skew (some events have 100x more registrations)
- Unnecessary shuffles (add broadcast joins for small tables)
- Caching intermediate DataFrames that are reused
- Predicate pushdown to skip irrelevant partitions
- Storage: switch from CSV to Parquet with snappy compression
- Executor config: driver/executor memory, parallelism tuning
```

### `@polars`
High-performance DataFrame processing with Polars (Rust-backed).
```
@polars Rewrite our slow pandas pipeline using Polars:

Current (slow):
import pandas as pd
df = pd.read_csv("registrations_2m.csv")  # 2M rows, takes 30s to load
grouped = df.groupby("event_id").agg({"amount": "sum", "id": "count"})

Rewrite with Polars lazy API:
- LazyFrame scan from Parquet (not CSV)
- Filter: only confirmed status
- Group by event_id
- Aggregate: sum amount, count registrations
- Sink to Parquet output
Show: code + performance benchmark comparison.
```

---

## 🏭 Data Warehousing

### `@snowflake-development`
Design Snowflake database structure, roles, and compute.
```
@snowflake-development Set up Snowflake for our events analytics:

Database structure:
- EVENTS_DB
  ├── RAW (raw ingested data, never modified)
  ├── STAGING (cleaned, typed data)
  └── MARTS (aggregated, business-ready tables)

Warehouse sizing:
- EVENTS_LOAD_WH (XS): for dbt runs and ETL loads
- EVENTS_QUERY_WH (S): for Metabase/dashboard queries
- EVENTS_ADHOC_WH (M, auto-suspend 60s): for analyst exploration

Roles:
- LOADER: can INSERT into RAW schema only
- TRANSFORMER: can SELECT from RAW, full access to STAGING/MARTS
- REPORTER: SELECT only on MARTS
- ANALYST: SELECT on STAGING + MARTS

Clustering: events fact table clustered on (event_date, category_id)
```

---

## ✅ Data Quality

### `@data-quality-frameworks`
Implement data quality checks throughout the pipeline.
```
@data-quality-frameworks Implement data quality for our events pipeline:

Checks to run after each load cycle:

Completeness:
- registrations.user_id is never null
- events.title has no empty strings

Freshness:
- max(registrations.created_at) < 90 minutes ago (for hourly pipeline)

Accuracy:
- events.capacity >= confirmed_registration_count (no over-registration)
- payments.amount > 0 for all paid events

Consistency:
- registration status distribution is similar to yesterday (no mass state change)

Alerting:
- Slack message: check name, table, rule, expected vs actual value, timestamp
- Block downstream DAGs if critical checks fail
```

### Monte Carlo Data Observability Suite

The `@monte-carlo-*` skills (9 total) are the gold standard for data observability in production pipelines:

| Skill | When to Use |
|---|---|
| `@monte-carlo-monitor-creation` | Set up automated anomaly detection monitors |
| `@monte-carlo-analyze-root-cause` | Investigate data quality incidents |
| `@monte-carlo-asset-health` | View overall health of a data asset |
| `@monte-carlo-monitoring-advisor` | Get recommendations for what to monitor |
| `@monte-carlo-performance-diagnosis` | Diagnose slow pipelines or queries |
| `@monte-carlo-prevent` | Proactively prevent data quality issues |
| `@monte-carlo-remediation` | Fix detected data quality issues |
| `@monte-carlo-storage-cost-analysis` | Analyze and reduce storage costs |
| `@monte-carlo-push-ingestion` | Monitor data ingestion in real time |

```
@monte-carlo-monitoring-advisor Our events pipeline feeds an executive dashboard.
Tables: events, registrations, payments, users
Key metrics: daily registration count, daily revenue, active event count

What should we monitor and with what thresholds?
Prioritize by business impact.
```

---

## 📊 Analytics & Visualization

### `@matplotlib` / `@seaborn` / `@plotly`
Python visualization for data exploration and reporting.
```
@plotly Build an interactive dashboard for event analytics:
Charts:
1. Registration trend: line chart, 30-day rolling, by category
2. Revenue by event: horizontal bar, sorted descending
3. Capacity utilization: heatmap (event × week)
4. Funnel: views → registrations → attended
Use Plotly Dash for a web-based interactive dashboard.
```

### `@scikit-learn`
ML models for data engineering use cases.
```
@scikit-learn Build a churn prediction model for event organizers:
Who is likely to stop creating events?
Features: events_created_30d, avg_attendance_rate, revenue_trend, last_active_days
Target: churned_90d (binary)
Model: XGBoost (start simple, interpretable)
Evaluation: precision/recall, ROC-AUC
Deployment: batch score weekly, flag at-risk organizers in CRM
```

---

## 🔗 Complete Data Engineering Prompt Chain

```
1️⃣  @data-engineering-data-pipeline
    "Design extraction strategy: CDC vs batch, source to staging"

2️⃣  @airflow-dag-patterns
    "Build orchestration DAGs with retries, SLAs, and alerting"

3️⃣  @dbt-transformation-patterns
    "Write staging → intermediate → mart models with tests"

4️⃣  @snowflake-development
    "Set up warehouse structure, roles, clustering, compute sizing"

5️⃣  @data-quality-frameworks
    "Add checks at each pipeline stage: completeness, freshness, accuracy"

6️⃣  @monte-carlo-monitoring-advisor
    "Get recommendations for anomaly detection monitors"

7️⃣  @monte-carlo-monitor-creation
    "Implement monitors on critical tables"

8️⃣  @plotly
    "Build interactive dashboard visualizing pipeline output"

9️⃣  @data-storytelling
    "Present findings to stakeholders with clear narrative"
```

---

## 💡 Pro Tips for Data Engineers

1. **Always design for idempotency** — pipelines will fail and need to re-run
2. **Schema-first with `@dbt-transformation-patterns`** — document models before coding
3. **`@monte-carlo-monitoring-advisor` before production** — know what to monitor
4. **`@polars` over pandas** for files > 500MB — 10x-100x faster
5. **`@data-quality-frameworks` in CI** — run quality checks as part of your DAG
