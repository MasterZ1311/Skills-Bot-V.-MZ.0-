# 🤖 Workflow 05 — Add AI Features to an Existing App

This workflow adds production-quality AI/ML capabilities to an existing application — from simple LLM integration to full RAG-based conversational search.

---

## 🎯 Goal
Integrate AI features that add real user value — not AI for AI's sake.

**Example:** Adding "Ask about events" AI assistant + smart recommendations to the college events platform

---

## ⏱️ Timeline

| Phase | Time |
|---|---|
| Feature Definition | 2-4 hours |
| Embedding Pipeline | 1 day |
| RAG System | 1-2 days |
| LLM Integration | 1 day |
| Recommendations | 1-2 days |
| Observability | Half a day |
| Production Hardening | Half a day |
| **Total** | **~5-8 days** |

---

## Phase 1 — Define the AI Feature

### Step 1.1 — Feature Scoping
```
@brainstorming We want to add AI to our college events platform.
Current product: students browse, filter, and register for events.
User pain: "I don't know which events are right for me" + "I can't describe what I'm looking for"

AI feature candidates:
1. Natural language search: "find free music events this weekend"
2. Personalized recommendations: "based on what you attended, you might like..."
3. Event description enhancement: organizers get AI-improved descriptions
4. Q&A assistant: "when does registration close for the hackathon?"
5. Auto-categorization: classify events on creation

Which features have the highest user value? Which are feasible in 1 week?
Evaluate each on: user value, technical complexity, data requirements, cost.
```

**Output:** Prioritized feature list, "start here" decision

### Step 1.2 — Choose the Right AI Approach
```
@llm-app-patterns We've chosen to build:
Feature 1: Natural language event search (semantic + filtered)
Feature 2: "You might also like" personalized recommendations

For Feature 1 (search):
- Option A: Pure LLM (send all events as context, ask LLM to pick)
  → Too expensive, won't scale beyond 100 events
- Option B: Embedding + vector search (embed query, find similar events)
  → Scales to millions of events, fast, cheap
- Option C: Hybrid (extract filters with LLM, execute structured search)
  → Best of both: LLM understands intent, DB handles filtering

Recommend Option C. Walk me through the implementation plan.

For Feature 2 (recommendations):
- Collaborative filtering: need lots of user history (we don't have it yet)
- Content-based: recommend events similar to ones user attended
- Simple popularity: recommend trending events as fallback

Recommend: content-based + popularity fallback for an early-stage product.
```

---

## Phase 2 — Embedding Pipeline

### Step 2.1 — Choose Embedding Model
```
@embedding-strategies Choose and implement embedding model for our events:
Events to embed: 10,000 events (title + description + tags)

Options:
A. OpenAI text-embedding-3-small
   - Quality: excellent for English
   - Cost: $0.02 per million tokens ≈ ₹0.02 for all 10,000 events
   - Latency: 200ms per batch

B. sentence-transformers/all-MiniLM-L6-v2 (free, self-hosted)
   - Quality: very good for English
   - Cost: free (run on our server or HuggingFace Inference API)
   - Latency: 50ms local, 300ms HuggingFace API

C. multilingual-e5-large (if we need regional languages)
   - Quality: supports Hindi and 100+ languages
   - Cost: free (run locally)

For our use case: Recommend A (OpenAI) for MVP quality, 
switch to B if cost becomes significant at scale.

Show: Python code to batch embed all events and upsert to pgvector.
```

**Output:** Embedding model choice, batch embedding script

### Step 2.2 — Vector Store Setup
```
@rag-engineer Set up pgvector for our event embeddings:
Using: PostgreSQL + pgvector extension (we already have Neon PostgreSQL)

Schema additions:
ALTER TABLE events ADD COLUMN embedding vector(1536); -- OpenAI ada dimensions
CREATE INDEX ON events USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100);

Also add metadata for filtered search:
- category: text (music, sports, academic, cultural, hackathon)
- date: timestamp
- is_free: boolean
- is_full: boolean
- college_id: uuid

Batch embedding script (Python):
1. Fetch all published events from DB
2. Batch 100 at a time → OpenAI embeddings API
3. UPSERT embeddings to events table
4. Schedule: run nightly for new events, webhook-trigger on event create/update

Show: complete Python script with error handling and progress bar.
```

**Output:** pgvector setup SQL, batch embedding script

### Step 2.3 — Hybrid Search Implementation
```
@hybrid-search-implementation Implement hybrid search in PostgreSQL:
Combine: full-text search (keyword matching) + vector search (semantic similarity)

Method: Reciprocal Rank Fusion (RRF)

SQL query structure:
WITH 
keyword_search AS (
  SELECT id, ts_rank(to_tsvector('english', title || ' ' || description), 
                     plainto_tsquery($1)) AS rank
  FROM events
  WHERE to_tsvector(...) @@ plainto_tsquery($1)
    AND status = 'published'
    AND date > NOW()
    -- Optional filters:
    AND ($2::text IS NULL OR category = $2)  -- category filter
    AND ($3::boolean IS NULL OR is_free = $3) -- free filter
  ORDER BY rank DESC
  LIMIT 20
),
vector_search AS (
  SELECT id, 1 - (embedding <=> $4::vector) AS similarity
  FROM events
  WHERE status = 'published' AND date > NOW()
    AND ($2::text IS NULL OR category = $2)
  ORDER BY embedding <=> $4::vector
  LIMIT 20
)
SELECT 
  e.*,
  COALESCE(1.0/(60 + ks.rank), 0) + COALESCE(1.0/(60 + vs.rank_pos), 0) AS rrf_score
FROM events e
LEFT JOIN keyword_search ks ON e.id = ks.id
LEFT JOIN (SELECT id, ROW_NUMBER() OVER(ORDER BY similarity DESC) as rank_pos FROM vector_search) vs 
          ON e.id = vs.id
WHERE ks.id IS NOT NULL OR vs.id IS NOT NULL
ORDER BY rrf_score DESC
LIMIT 5;

Parameters: $1=keywords, $2=category, $3=is_free, $4=query_embedding
```

---

## Phase 3 — LLM Integration (Intent Extraction)

### Step 3.1 — Query Intent Extraction
```
@llm-structured-output Implement intent extraction from natural language queries:
Input: "free music events this weekend"
Output: { category: "music", is_free: true, date_from: "2025-12-13", date_to: "2025-12-15" }

Implementation using Zod + Vercel AI SDK:
import { generateObject } from 'ai';
import { openai } from '@ai-sdk/openai';
import { z } from 'zod';

const SearchFilters = z.object({
  keywords: z.string().describe("Key search terms, cleaned of filter words"),
  category: z.enum(["music", "sports", "academic", "cultural", "hackathon"]).nullable(),
  is_free: z.boolean().nullable().describe("True if user wants only free events"),
  date_from: z.string().nullable().describe("ISO date string for start date filter"),
  date_to: z.string().nullable().describe("ISO date string for end date filter"),
  sort_by: z.enum(["relevance", "date_asc", "popularity"]).default("relevance")
});

async function extractSearchIntent(query: string): Promise<SearchFilters> {
  const { object } = await generateObject({
    model: openai('gpt-4o-mini'), // cheapest model, simple task
    schema: SearchFilters,
    prompt: `Extract search intent from: "${query}". 
             Today is ${new Date().toISOString()}. 
             "this weekend" = next Saturday and Sunday.`
  });
  return object;
}
```

**Output:** Intent extraction function with type-safe output

### Step 3.2 — Embed User Query
```
@rag-engineer Implement the complete search flow:

async function searchEvents(naturalLanguageQuery: string) {
  // Step 1: Extract structured intent from query
  const intent = await extractSearchIntent(naturalLanguageQuery);
  
  // Step 2: Embed the cleaned keywords for semantic search
  const embedding = await embedText(intent.keywords);
  
  // Step 3: Execute hybrid search with filters
  const results = await db.hybridSearch({
    keywords: intent.keywords,
    embedding,
    category: intent.category,
    is_free: intent.is_free,
    date_from: intent.date_from,
    date_to: intent.date_to,
    limit: 5
  });
  
  // Step 4: If 0 results, broaden search (remove date filter first)
  if (results.length === 0 && intent.date_from) {
    return await db.hybridSearch({ ...without date filters..., limit: 5 });
  }
  
  return results;
}
```

### Step 3.3 — Conversational Interface
```
@langchain-architecture Build the conversational events Q&A interface:
Using: Vercel AI SDK (streamText) + LangChain for retrieval

System prompt:
"You are a helpful campus events assistant. 
Answer questions about upcoming events accurately. 
Only discuss events that exist in the retrieved context. 
If no relevant events found, say so honestly.
Format dates as: 'Saturday, December 15th at 6:00 PM'.
Keep answers concise: 2-3 sentences max."

Tool: search_events (calls our hybrid search function)

Streaming response:
- API route: POST /api/ai/chat
- Use: streamText from Vercel AI SDK
- Frontend: useChat hook from ai/react
- Show: typing indicator while streaming, smooth token reveal
- Error: graceful fallback if LLM unavailable ("Search is temporarily unavailable")

Rate limiting: 20 questions per user per hour (prevent abuse + cost control).
```

---

## Phase 4 — Personalized Recommendations

### Step 4.1 — User Interest Profiling
```
@embedding-strategies Build user interest profiles from behavior:
Signals to track (in order of strength):
1. Registered (attended): strongest signal → user likes this type of event
2. Viewed detail page: moderate signal → user was interested
3. Selected category filter: mild signal → user browsed this category
4. Searched for category: mild signal

User embedding:
- Aggregate embeddings of events user registered for (weighted average)
- Weight: registered (1.0) > viewed (0.3) > filtered (0.1)
- Recency weight: last 30 days > last 90 days

Store: user_interests table
- user_id, interest_vector (pgvector), last_updated

Update: when user registers for event or views event detail (async, not on critical path).
```

### Step 4.2 — Recommendation Engine
```
@data-engineering-data-driven-feature Build the recommendation query:

SELECT e.*, 
       1 - (e.embedding <=> u.interest_vector) AS relevance_score
FROM events e
CROSS JOIN (SELECT interest_vector FROM user_interests WHERE user_id = $1) u
WHERE e.status = 'published'
  AND e.date > NOW()
  AND e.id NOT IN (
    SELECT event_id FROM registrations WHERE user_id = $1
  )  -- Don't recommend events user already registered for
ORDER BY e.embedding <=> u.interest_vector
LIMIT 5;

Fallback (new user with no history):
SELECT e.*, r.registration_count
FROM events e
JOIN (
  SELECT event_id, COUNT(*) as registration_count 
  FROM registrations WHERE status = 'confirmed'
  GROUP BY event_id
) r ON e.id = r.event_id
WHERE e.status = 'published' AND e.date > NOW()
ORDER BY r.registration_count DESC
LIMIT 5;
```

**Output:** Recommendation SQL + fallback logic

### Step 4.3 — API Integration
```
@nextjs-app-router-patterns Add recommendations to the home feed:
Route: GET /api/recommendations

Logic:
1. If user has interest_vector: return personalized results
2. Else: return popularity-based results
3. Deduplicate with events already on the page

Frontend integration:
- Home page: "Recommended for you" section (client component)
- Initial SSR: include 5 recommendations in page payload
- Refresh: re-fetch after user registers for event (invalidate query)
- UI: EventCard grid with "Because you attended [Event]" label

Cache: recommendations cached per user for 1 hour in Redis.
```

---

## Phase 5 — AI Content Enhancement

### Step 5.1 — Event Description Enhancer
```
@prompt-engineering Build the event description enhancer for organizers:
Feature: after organizer writes description, offer "Enhance with AI" button

Prompt design:
System: "You are an expert event copywriter for college events.
Enhance event descriptions to be compelling and clear.
Keep it under 200 words. 
Preserve all factual information (dates, venues, speakers, prizes).
Do not add information that wasn't in the original."

User: "Original description: [organizer's text]
Event type: [category]
Target audience: college students

Write an enhanced version."

Implementation:
- Button: "✨ Enhance with AI" below description textarea
- Loading: show shimmer over textarea while generating
- Result: show diff (original vs enhanced) side by side
- Accept/Reject: organizer chooses which version to use
- Track: how often organizers accept AI suggestions (product metric)
```

---

## Phase 6 — Observability

### Step 6.1 — LLM Tracing with Langfuse
```
@langfuse Instrument our AI features with Langfuse:

Traces to capture:
1. Search query trace:
   - Input: natural language query
   - Span: extractSearchIntent (model=gpt-4o-mini, tokens, latency)
   - Span: embedText (model=text-embedding-3-small, tokens, latency)
   - Span: hybridSearch (SQL, results count, latency)
   - Output: final search results

2. Chat response trace:
   - Input: user message + conversation history
   - Span: retrieval (query, results)
   - Span: generation (model, tokens, latency, streaming)
   - Output: assistant response

3. Description enhancement trace:
   - Input: original description
   - Span: generation
   - Output: enhanced description
   - User feedback: accepted/rejected

Metrics to track in Langfuse:
- Search: 0 results rate (measure intent extraction accuracy)
- Chat: average tokens per response (cost optimization)
- Enhancement: acceptance rate
- All: latency P50/P95, error rate
```

### Step 6.2 — Cost Monitoring
```
@langfuse Set up cost monitoring for our AI spend:
Budget: ₹5,000/month for AI API costs

Cost per feature:
- Search query: 2 LLM calls (intent + sometimes rephrase) + 1 embedding
  ≈ $0.001 per search = ₹0.08 per search
  At 1,000 searches/day = ₹2,400/month

- Chat message: 1 LLM call
  ≈ $0.003 per message = ₹0.25 per message
  At 500 messages/day = ₹3,750/month

Total at current scale: ~₹6,000/month → over budget

Optimizations:
- Cache embeddings (never re-embed unchanged events)
- Rate limit: 20 searches per user per day
- Use gpt-4o-mini for intent extraction (gpt-4o is overkill)
- Cache common query intents: "free events" always extracts the same filters

Monitor: daily spend alert if > ₹200/day (to catch runaway usage).
```

---

## Phase 7 — Production Hardening

### Step 7.1 — Guardrails & Safety
```
@llm-app-patterns Add guardrails to our AI features:

Input validation:
- Max query length: 500 chars (prevent prompt injection via long inputs)
- Detect prompt injection: if query contains "ignore previous instructions" → reject
- Profanity filter: block inappropriate queries

Output validation:
- Never return events that don't exist in our database
- Verify: every recommended event has a valid ID that exists in DB
- No PII in responses: if LLM hallucinate a person's email → strip it

Fallback chain:
1. Primary: OpenAI GPT-4o-mini for intent extraction
2. Fallback: if OpenAI unavailable → regex-based filter extraction
3. Fallback: if regex fails → return all events sorted by date

Monitoring:
- Alert: if AI error rate > 5% → PagerDuty
- Alert: if LLM returns obvious hallucination (event not in DB)
```

### Step 7.2 — A/B Test the AI Feature
```
@posthog-automation A/B test our AI search vs standard search:
Using PostHog feature flags

Variant A (control): existing keyword search bar
Variant B (treatment): "Ask me anything" AI search box

Allocation: 50/50 split, eligible users only
Metrics to compare:
- Primary: events registered per session (does AI search lead to more registrations?)
- Secondary: search success rate (% of searches that resulted in a page view)
- Tertiary: session length (do AI users spend more time exploring?)

Run for: 2 weeks minimum, target 200 users per variant
Analyze: with PostHog Experiment feature, check for statistical significance (p < 0.05).
If treatment wins: roll out to 100%
If control wins: AI search not valuable → pivot or improve
```

---

## ✅ AI Feature Launch Checklist

```
Infrastructure:
□ pgvector extension enabled on PostgreSQL
□ Event embeddings generated for all published events
□ Nightly embedding update job scheduled
□ Redis cache configured for recommendation results

Features:
□ Natural language search: working with intent extraction + hybrid retrieval
□ "0 results" fallback: broadens search automatically
□ Recommendations: personalized for users with history, popularity-based for new
□ Description enhancer: working with accept/reject UI

Quality:
□ Manual testing: 50 diverse queries tested, results make sense
□ Edge cases: empty query, very long query, non-English query
□ No hallucinations in chat responses (only answers from retrieved context)

Cost & Safety:
□ Rate limiting: 20 AI searches per user per day
□ Spend monitoring: Langfuse daily cost dashboard
□ Fallback working: tested by disabling OpenAI API key
□ Input sanitization: prompt injection attempts blocked

Observability:
□ Langfuse tracing: every AI call traced
□ Alerts: error rate > 5% → notification
□ A/B test: configured in PostHog, metrics defined

Compliance:
□ AI disclosure: users informed they're interacting with AI
□ Data: user queries not used for model training (check OpenAI ToS)
```
