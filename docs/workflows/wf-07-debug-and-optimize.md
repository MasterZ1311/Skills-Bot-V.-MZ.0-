# 🔎 Workflow 07 — Debug, Profile & Optimize a Slow App

This workflow systematically finds performance bottlenecks and fixes them — from API latency to frontend rendering, database queries, and memory leaks.

---

## 🎯 Goal
Identify root causes of performance problems and fix them with measurable improvements.

**Example:** College events platform experiencing slow registration flow

---

## ⏱️ Timeline

| Phase | Time |
|---|---|
| Measure & Baseline | 2-4 hours |
| API Profiling | 4-8 hours |
| Database Optimization | 1-2 days |
| Frontend Optimization | 1-2 days |
| Infrastructure Tuning | Half a day |
| Load Testing & Verification | Half a day |
| **Total** | **~4-6 days** |

---

## Phase 1 — Measure First, Optimize Later

> **Rule #1: Never optimize without data.** Gut feelings about bottlenecks are wrong ~70% of the time.

### Step 1.1 — Establish Baseline Metrics
```
@systematic-debugging Establish a performance baseline before optimizing:
Our symptoms: 
- Registration API: P99 latency 3.2 seconds (target: < 500ms)
- Home page: Largest Contentful Paint 4.8s (target: < 2.5s)
- Database: occasional 5-10 second queries under load

Measurement tools to set up:
1. API timing: Datadog APM or OpenTelemetry traces (if not already set up)
   - Per-endpoint P50/P95/P99 latency
   - Breakdown: auth middleware + handler + DB + downstream calls

2. Database: pg_stat_statements
   - Enable: CREATE EXTENSION pg_stat_statements
   - View: SELECT * FROM pg_stat_statements ORDER BY total_time DESC LIMIT 20

3. Frontend: Lighthouse CI
   - Run: npx lighthouse-ci https://staging.events.college.edu
   - Metrics: LCP, TBT, CLS, Speed Index
   - Run on every PR (lighthouse-ci GitHub Action)

4. Load: k6 baseline test (10 users, 5 minutes)
   - Measure: requests/second, error rate, P95 latency
   
Document: current state before any changes. This is your before/after comparison.
```

**Output:** Baseline metrics document, prioritized bottleneck list

### Step 1.2 — Distributed Tracing Setup
```
@opentelemetry-setup Set up OpenTelemetry tracing for the full request path:
Instrument: Next.js API routes → Prisma → any external calls

@opentelemetry/sdk-node + @opentelemetry/auto-instrumentations-node
Exporter: OTLP → Jaeger (local) or Grafana Tempo (production)

What to trace:
- Every API request: HTTP method, path, status, duration
- Every DB query: SQL text (sanitized), table, duration
- Every external call: Stripe, email, Redis

Span annotations to add manually:
- "events.search.results_count": number of results returned
- "registration.capacity_check": remaining spots value
- "payment.amount": charge amount in INR

View: trace for a single slow registration request — which span is longest?
```

---

## Phase 2 — API & Backend Profiling

### Step 2.1 — Node.js CPU Profiling
```
@debugging-toolkit Profile our Node.js API for CPU hotspots:
Symptom: API is slow under load even with DB queries fast (suspected CPU bottleneck)

Method 1: Clinic.js
npx clinic doctor -- node server.js
# Generates flame graph showing where CPU time is spent

Method 2: Node.js built-in inspector
node --inspect server.js
# Connect Chrome DevTools → Performance tab → Record → load test → analyze

Method 3: 0x (flamegraph tool)
npx 0x -o server.js
# Visual flame graph: which functions consume most CPU?

Look for:
- Unexpectedly wide frames: computationally expensive functions
- JSON.parse/stringify on large payloads: optimize or avoid
- Sync operations in async handlers: readline, crypto (use async versions)
- Regular expressions on large strings: potential ReDoS
- Unintended serialization: circular references forcing large object copies
```

### Step 2.2 — Memory Profiling
```
@debugging-toolkit Find and fix memory leaks in our API server:
Symptom: memory grows from 150MB to 800MB over 6 hours → restart required

Detection:
1. Monitor: track process.memoryUsage().heapUsed every minute
   → Is it growing monotonically? (If yes: leak. If oscillating: normal GC)

2. Heap snapshot:
   v8.writeHeapSnapshot() → load in Chrome DevTools Memory tab
   Take 2 snapshots (1 hour apart) → compare → what grew?

Common leaks in Node.js:
- Global variables accumulating (event listeners, caches without TTL)
- Closures holding references to large objects
- EventEmitter listeners not removed (process.on('uncaughtException') added in loop)
- setTimeout/setInterval not cleared

Fix template:
// LEAK: global cache growing unbounded
const cache = {}; // never cleaned
// FIX: use LRU cache with max size
import { LRUCache } from 'lru-cache';
const cache = new LRUCache({ max: 500 });
```

### Step 2.3 — Async & I/O Optimization
```
@performance-patterns Identify and fix async anti-patterns:

Anti-pattern 1: Sequential awaits that could be parallel
// SLOW: 3 DB calls in sequence (600ms total)
const event = await db.events.findUnique({ where: { id } });
const registrationCount = await db.registrations.count({ where: { eventId: id } });
const userRegistration = await db.registrations.findFirst({ where: { userId, eventId: id } });

// FAST: parallel (200ms total — only as slow as the slowest one)
const [event, registrationCount, userRegistration] = await Promise.all([
  db.events.findUnique({ where: { id } }),
  db.registrations.count({ where: { eventId: id } }),
  db.registrations.findFirst({ where: { userId, eventId: id } })
]);

Anti-pattern 2: N+1 queries in loops
// SLOW: 1 query per event in the list (N events = N queries)
const events = await db.events.findMany({ take: 20 });
for (const event of events) {
  event.registrationCount = await db.registrations.count({ where: { eventId: event.id } });
}

// FAST: include count in original query (2 total queries max)
const events = await db.events.findMany({
  take: 20,
  include: { _count: { select: { registrations: true } } }
});

Ask @n-plus-one-query-detector to scan our entire codebase for this pattern.
```

---

## Phase 3 — Database Optimization

### Step 3.1 — Slow Query Analysis
```
@database-query-optimizer Analyze and fix slow queries:
Source: pg_stat_statements + EXPLAIN ANALYZE

Step 1: Find top 10 slow queries
SELECT 
  query,
  calls,
  total_time / calls AS avg_ms,
  rows / calls AS avg_rows,
  total_time
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;

Step 2: For each slow query, run EXPLAIN ANALYZE:
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT e.*, COUNT(r.id) as reg_count
FROM events e
LEFT JOIN registrations r ON r.event_id = e.id
WHERE e.status = 'published'
  AND e.date > NOW()
  AND e.category = 'music'
GROUP BY e.id
ORDER BY e.date ASC
LIMIT 20;

Look for in the plan:
- "Seq Scan" on large tables → needs index
- High "rows removed by filter" → index is wrong or missing
- Nested Loop on large sets → may need Hash Join hint or index
- "Sort" on large intermediate result → add index for ORDER BY column
```

### Step 3.2 — Index Optimization
```
@database-index-advisor Add missing indexes for our query patterns:
Common queries to index for:

1. Events listing with filters (most common):
CREATE INDEX CONCURRENTLY idx_events_status_date 
ON events (status, date)
WHERE status = 'published'; -- partial index (only published events)

2. Events by category + date (filtered browse):
CREATE INDEX CONCURRENTLY idx_events_category_date
ON events (category, date)
WHERE status = 'published' AND date > NOW();
-- Note: NOW() in index definition doesn't work — use partial index without NOW()

3. Registration lookup (critical path):
CREATE INDEX CONCURRENTLY idx_registrations_user_event
ON registrations (user_id, event_id); -- covers "did this user register?"
CREATE INDEX CONCURRENTLY idx_registrations_event_status
ON registrations (event_id, status); -- covers capacity count

4. Ticket lookup:
CREATE INDEX CONCURRENTLY idx_tickets_registration
ON tickets (registration_id);

5. Waitlist position:
CREATE INDEX CONCURRENTLY idx_waitlist_event_position
ON waitlist (event_id, position);

After adding: re-run EXPLAIN ANALYZE and confirm "Index Scan" instead of "Seq Scan".
```

### Step 3.3 — Connection Pool Tuning
```
@database-connection-pool Tune PostgreSQL connection pool:
Current: Prisma default (10 connections) → maxing out under load

Diagnosis:
SELECT count(*), state 
FROM pg_stat_activity 
GROUP BY state;
-- If active > pool size: requests are queuing

Prisma pool configuration:
// prisma.ts
const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL + "?connection_limit=20&pool_timeout=30"
    }
  }
});

For serverless (Vercel / Lambda):
- Use PgBouncer (connection pooler) between app and RDS
- Vercel Postgres / Neon: have built-in pooling
- Set connection_limit=1 per serverless function (they're short-lived)

PostgreSQL max_connections:
- t3.medium RDS: ~170 max connections
- Reserve 10 for admin
- Our app: 20 connections × 3 instances = 60 connections
- Leaves room for growth and DBA access

Monitor: pg_stat_activity — alert if active connections > 80% of pool size.
```

### Step 3.4 — Query Caching
```
@redis-caching-patterns Add caching for expensive, frequently-read queries:
Cache layer: Redis (Upstash for serverless, AWS ElastiCache for dedicated)

What to cache:
1. Events listing (varies by filters):
   Key: events:published:music:2025-12 (category + month)
   TTL: 5 minutes (acceptable staleness for browse)
   Invalidate: when any event in that category is created/updated

2. Single event detail:
   Key: event:{id}
   TTL: 1 minute
   Invalidate: when that event is updated

3. Registration count per event:
   Key: event:{id}:reg_count
   TTL: 30 seconds (must be fresh for capacity check display)
   Strategy: increment/decrement on registration create/cancel instead of TTL

4. User's registrations list:
   Key: user:{id}:registrations
   TTL: 5 minutes
   Invalidate: when user registers or cancels

Cache implementation (Redis + Next.js):
import { Redis } from '@upstash/redis';
const redis = Redis.fromEnv();

async function getCachedEvents(filters: Filters): Promise<Event[]> {
  const key = `events:${JSON.stringify(filters)}`;
  const cached = await redis.get(key);
  if (cached) return cached as Event[];
  
  const events = await db.events.findMany(toQuery(filters));
  await redis.set(key, events, { ex: 300 }); // 5 min TTL
  return events;
}
```

---

## Phase 4 — Frontend Optimization

### Step 4.1 — Core Web Vitals
```
@web-performance-optimization Fix Core Web Vitals for our events platform:
Measured with Lighthouse (current scores):
- LCP: 4.8s ❌ (target: < 2.5s) — main culprit: event hero image
- INP: 380ms ⚠️  (target: < 200ms) — register button click
- CLS: 0.22 ❌ (target: < 0.1) — images without size attributes

Fix LCP (hero image):
1. Add width + height to all <img> tags (prevents layout shift too)
2. Add fetchPriority="high" to above-fold event images
3. Use Next.js <Image> with proper sizing and formats (WebP/AVIF)
4. Use next/headers preload link for hero image
5. Host images on Vercel CDN (not Supabase) for edge caching

Fix INP (register button):
- Profile: which JS runs on button click? (Chrome DevTools Performance tab)
- Heavy computation on click? Move to Web Worker
- Stripe.js loading blocking click response? Lazy load non-critical scripts
- React component re-rendering unnecessarily? Add React.memo / useMemo

Fix CLS (layout shift):
- Add width + height to all images in JSX
- Reserve space for dynamic content (skeleton loaders with fixed height)
- Avoid injecting content above existing content after page load
```

### Step 4.2 — Bundle Optimization
```
@nextjs-performance Optimize our Next.js bundle:
Run: npx @next/bundle-analyzer (ANALYZE=true npm run build)

Find and fix:
1. Large dependencies:
   - moment.js (300KB) → replace with date-fns or dayjs (10-20KB)
   - lodash (full package, 68KB) → import specific: import debounce from 'lodash/debounce'
   - @stripe/stripe-js (60KB) → lazy load, only on checkout pages

2. Unnecessary imports in server components:
   - Any client library imported in server component?
   - Mark client components with "use client" precisely — don't over-apply

3. Code splitting:
   - Heavy components (RegistrationModal, QRScanner, MapView): 
     dynamic(() => import('./component'), { loading: () => <Skeleton /> })

4. Third-party scripts:
   - Google Analytics, Intercom, Stripe: load with next/script strategy="lazyOnload"
   - Never block render for analytics

Target: < 100KB First Load JS per route.
```

### Step 4.3 — React Performance
```
@react-performance-optimization Fix React rendering performance:
Profiling: React DevTools Profiler → record while scrolling events list → find re-renders

Common issues + fixes:

1. EventCard re-rendering when parent state changes:
// BEFORE: re-renders every parent state change
function EventCard({ event }) { ... }

// AFTER: only re-renders when event prop changes
const EventCard = memo(function EventCard({ event }) { ... });

2. Expensive computation on every render:
// BEFORE: re-calculates every render
const sortedEvents = events.sort((a, b) => a.date - b.date);

// AFTER: only recalculates when events changes
const sortedEvents = useMemo(
  () => [...events].sort((a, b) => a.date - b.date), 
  [events]
);

3. Callback references causing child re-renders:
// BEFORE: new function reference every render → EventCard re-renders
<EventCard onClick={() => handleClick(event.id)} />

// AFTER: stable reference
const handleClick = useCallback((eventId) => { ... }, []);
<EventCard onClick={handleClick} />

4. List virtualization (events list with 1000 items):
import { VirtualList } from '@tanstack/react-virtual';
// Only renders visible items (30-40) instead of all 1000
```

### Step 4.4 — Image Optimization
```
@nextjs-performance Optimize images across our platform:
Current: PNG images from Supabase Storage, ~500KB per event banner

Pipeline:
1. On upload: resize server-side before storing
   - Max dimensions: 1200×675px (16:9 for social sharing)
   - Convert to WebP: 80% quality (typically 5-10x smaller than PNG)
   - Generate thumbnail: 400×225px for card views

2. In Next.js <Image>:
   - Use sizes prop: sizes="(max-width: 768px) 100vw, 50vw"
   - Next.js generates: srcset with multiple sizes
   - Format: Next.js auto-serves WebP to supporting browsers

3. Blur placeholder:
   - Generate: blurDataURL on server (tiny 10x10 base64 image)
   - Show during loading: visual improvement without layout shift

4. CDN caching:
   - Vercel CDN: Next.js images cached at edge automatically
   - Cache-Control: max-age=31536000 (1 year, images are hash-named)

Expected improvement: LCP from 4.8s → ~1.5s
```

---

## Phase 5 — Infrastructure Tuning

### Step 5.1 — Serverless Function Optimization
```
@vercel-performance Optimize Vercel serverless functions:
Problem: cold starts adding 2-3 seconds latency for first user of the day

Cold start analysis:
1. Measure: check Vercel function logs for "cold start" events
2. Size: Function size directly impacts cold start time
   - Run: vercel build → check .vercel/output/functions size
   - Target: < 50MB per function

Optimizations:
1. Reduce function size:
   - Move heavy dependencies to edge-compatible packages
   - Use tree-shaking: avoid importing entire libraries

2. Edge runtime (fastest, no cold start):
   - Eligible routes: auth checks, simple reads, redirects
   - Not eligible: routes using native Node.js APIs or Prisma (use Node runtime)
   - Add: export const runtime = 'edge' to eligible routes

3. Prisma edge:
   - Use Prisma Accelerate (Prisma's edge-compatible proxy)
   - Or: use HTTP-based DB client (Neon HTTP client) for edge functions

4. Streaming responses:
   - Long API calls: stream the response instead of waiting for all data
   - User sees first data faster even if total time is the same
```

---

## Phase 6 — Load Testing & Verification

### Step 6.1 — Load Test After Optimizations
```
@k6-load-testing Run load tests to verify improvements:
Baseline test (run before and after optimizations):

import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  scenarios: {
    // Scenario 1: normal load
    normal: {
      executor: 'ramping-vus',
      startVUs: 0,
      stages: [
        { duration: '2m', target: 50 },   // ramp to 50 users
        { duration: '5m', target: 50 },   // steady state
        { duration: '2m', target: 0 },    // ramp down
      ]
    },
    // Scenario 2: spike (event registration opens)
    spike: {
      executor: 'ramping-vus', 
      startTime: '10m',
      stages: [
        { duration: '30s', target: 300 }, // instant spike
        { duration: '2m', target: 300 },  // sustained spike
        { duration: '30s', target: 0 },
      ]
    }
  },
  thresholds: {
    http_req_duration: ['p(95)<500'],  // 95% under 500ms
    http_req_failed: ['rate<0.01'],    // error rate < 1%
  }
};

export default function () {
  // Simulate registration flow
  const eventRes = http.get(`${BASE_URL}/api/events`);
  check(eventRes, { 'events list 200': (r) => r.status === 200 });
  
  const regRes = http.post(`${BASE_URL}/api/registrations`, 
    JSON.stringify({ eventId: 'test-event-1', ticketCount: 1 }),
    { headers: { 'Content-Type': 'application/json', 'Authorization': `Bearer ${TOKEN}` } }
  );
  check(regRes, { 'registration 201': (r) => r.status === 201 });
  
  sleep(1);
}

Compare: before vs after optimization results.
Target: P95 under 500ms at 100 concurrent users.
```

### Step 6.2 — Verify No Regressions
```
@playwright-skill Run full E2E test suite after optimizations:
Purpose: confirm that performance optimizations didn't break functionality

Critical paths to test:
1. Event discovery: browse, filter, search — all returning correct results
2. Registration: complete payment flow — ticket issued correctly
3. Admin: view registrations — correct data shown
4. QR validation: scanner marks ticket as used

Performance assertions in Playwright:
test('registration page loads fast', async ({ page }) => {
  const startTime = Date.now();
  await page.goto('/events/hackathon-2025');
  await page.waitForSelector('[data-testid="register-button"]');
  const loadTime = Date.now() - startTime;
  expect(loadTime).toBeLessThan(2000); // page ready in < 2s
});

After passing: deploy to staging → measure with Lighthouse again.
Before/after report: share with team to confirm improvements.
```

---

## 📊 Performance Optimization Log

Track every optimization you make:

```
@debugging-toolkit Maintain a performance optimization log:
Format for each optimization:
| Date | What Changed | Before | After | Impact |
|------|-------------|--------|-------|--------|
| 2025-12-15 | Added index on events(status, date) | 450ms avg query | 12ms avg query | 97% faster |
| 2025-12-15 | Parallelized DB calls in event detail handler | 800ms P95 | 250ms P95 | 69% faster |
| 2025-12-16 | Image WebP conversion + resize | LCP 4.8s | LCP 1.9s | 60% faster |
| 2025-12-16 | Memoized EventCard component | 120ms render | 8ms render | 93% faster |
| 2025-12-17 | Redis cache on events listing | 400ms API | 8ms cached | 98% faster |

Summary after all optimizations:
- Registration API: P99 3,200ms → P99 380ms (88% improvement) ✅
- Home page LCP: 4.8s → 1.9s (60% improvement) ✅
- Events listing: 400ms → 8ms cached (98% improvement) ✅
- Error rate under load: 2.3% → 0.1% ✅
```

---

## ✅ Performance Optimization Checklist

```
Measurement:
□ Baseline metrics documented: API latency, DB query time, LCP, load test results
□ Tracing enabled: every slow request can be traced end-to-end
□ pg_stat_statements enabled: top slow queries visible

Database:
□ EXPLAIN ANALYZE run on all queries taking > 50ms
□ Indexes added for all frequent query patterns
□ N+1 queries eliminated (checked with @n-plus-one-query-detector)
□ Connection pool sized correctly for environment
□ Caching added for expensive, frequently-read queries

Backend:
□ Sequential awaits converted to Promise.all where independent
□ No CPU-intensive sync operations blocking event loop
□ Memory stable under load (no leaks — 1 hour test passed)
□ Error handling doesn't cause unnecessary retries

Frontend:
□ LCP < 2.5s on mobile (Lighthouse CI passing)
□ INP < 200ms (measured on real device)
□ CLS < 0.1 (no layout shifts)
□ Bundle < 100KB First Load JS per route
□ Images: WebP format, proper sizing, Next.js <Image>
□ React: heavy components memoized, no unnecessary re-renders

Verification:
□ Load test: P95 < 500ms at target concurrent users
□ Spike test: no errors during 3x normal load spike
□ E2E tests: all passing after optimizations
□ Before/after report: documented and shared
```
