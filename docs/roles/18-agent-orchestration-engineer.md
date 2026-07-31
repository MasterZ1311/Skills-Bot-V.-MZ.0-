# 🤝 Agent Orchestration Engineer — Skills Guide

Agent orchestration engineers design and operate multi-agent AI systems — coordinating specialized sub-agents, managing memory, building tools, and ensuring reliable agentic workflows.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Multi-Agent Architecture | `@multi-agent-architect`, `@multi-agent-patterns`, `@agent-orchestrator` |
| Parallel Execution | `@dispatching-parallel-agents`, `@parallel-agents`, `@subagent-orchestrator` |
| Memory Systems | `@agent-memory`, `@agent-memory-systems`, `@hierarchical-agent-memory`, `@context-manager` |
| Agent Workflows | `@antigravity-workflows`, `@agentflow`, `@goal-loop`, `@ai-loop` |
| Tool Building | `@agent-tool-builder`, `@mcp-builder`, `@mcp-tool-developer` |
| Skill Management | `@manage-skills`, `@skill-router`, `@skill-suggester`, `@skill-creator` |
| Debugging Agents | `@agenttrace-session-audit`, `@agentic-actions-auditor`, `@evaluation` |
| Context Management | `@context-window-management`, `@conversation-memory`, `@multi-agent-task-orchestrator` |

---

## 🏗️ Multi-Agent Architecture

### `@multi-agent-architect`
Design a system where multiple specialized agents collaborate.
```
@multi-agent-architect Design a multi-agent system for our events platform operations:

Goal: automate the end-to-end event lifecycle management

Agents needed:
1. EventDiscoveryAgent: scans college websites and social media for unregistered events
2. ContentEnhancerAgent: rewrites event descriptions to be compelling and SEO-optimized
3. ImageGeneratorAgent: creates banner images for events without photos
4. RegistrationMonitorAgent: watches for sold-out events and notifies waitlist
5. AnalyticsAgent: generates daily event performance reports
6. OrchestratorAgent: coordinates all above agents, handles errors, retries

For each agent:
- Input/output contracts
- Tools they need access to
- When they hand off to the next agent
- Failure handling

Communication: event-driven via message queue vs direct RPC? Recommend and justify.
```

### `@multi-agent-patterns`
Proven patterns for agent collaboration.
```
@multi-agent-patterns We need to process 1,000 event descriptions through 3 sequential agents:
1. ContentReviewAgent: checks for policy violations, rates quality (1-5)
2. EnhancementAgent: rewrites low-quality descriptions (score < 3)
3. SEOAgent: adds keywords and meta description

Choose the right pattern:
- Sequential chain: A → B → C (serial, slow for 1K items)
- Parallel fan-out: run all 1K through Agent 1 concurrently
- Conditional routing: skip Agent 2 if quality score ≥ 3
- MapReduce: process in batches of 50, aggregate results

Show the orchestration code for the chosen pattern.
```

### `@dispatching-parallel-agents` / `@parallel-agents`
Run multiple agents concurrently for speed.
```
@dispatching-parallel-agents Process 50 event pages simultaneously:
Task: for each of 50 upcoming events, generate:
- Enhanced description (ContentEnhancerAgent)
- SEO metadata (SEOAgent)
- Banner image prompt (ImagePromptAgent)

Dispatch pattern:
- Create 50 tasks
- Semaphore: max 10 concurrent to avoid rate limits
- Collect results: wait for all 50 to complete
- Error handling: retry failed tasks up to 3 times
- Progress tracking: show % complete in real-time

Show: Python asyncio implementation with asyncio.gather + Semaphore.
```

---

## 🧠 Memory Systems

### `@agent-memory` / `@agent-memory-systems`
Design memory architecture for persistent, contextual agents.
```
@agent-memory Design a memory system for our EventAssistantAgent:
The agent helps students find events and answers questions over multiple sessions.

Memory layers needed:
1. Working memory (in-context):
   - Current conversation (last 10 turns)
   - Current search results
   - User's current intent

2. Episodic memory (recent sessions):
   - Last 5 conversation summaries
   - Events the user viewed/registered for
   - Questions the user asked previously
   Storage: Redis with TTL (30 days)

3. Semantic memory (user profile):
   - User's interest categories (learned over time)
   - Preferred event types and times
   - Colleges they're associated with
   Storage: PostgreSQL user_preferences table

4. Procedural memory (how to do things):
   - How to search events (tool definitions)
   - How to register (workflow steps)
   Storage: skill files loaded at agent startup

Show: memory retrieval and update logic in Python.
```

### `@hierarchical-agent-memory`
Multi-level memory for complex agent systems.
```
@hierarchical-agent-memory Design hierarchical memory for our multi-agent events system:

Level 1 — Global (shared across all agents):
- All current active events database
- Campus map and venue information
- Platform policies and rules
- Tool definitions

Level 2 — Session (per user conversation):
- Current conversation context
- User's stated preferences in this session
- Events browsed and actions taken

Level 3 — Task (per specific task):
- Current search query and results
- Candidate events under consideration
- User feedback on suggestions

Memory propagation:
- Task memory summarized → Session memory after task complete
- Session memory summarized → Global user profile after conversation ends
- How to avoid memory poisoning (irrelevant context accumulating)
```

### `@context-window-management`
Stay within LLM context limits while maintaining coherence.
```
@context-window-management Our EventAssistantAgent hits context limit after ~20 turns:
Context window: 128K tokens (GPT-4o)
Current usage breakdown:
- System prompt: 2,000 tokens
- Tool definitions: 3,000 tokens
- Conversation history: growing unboundedly

Strategy:
- Sliding window: keep only last 10 turns in full
- Summarization: beyond 10 turns, summarize older turns (LLM call)
- Important event extraction: always keep events the user showed interest in
- Tool call compression: summarize tool results > 1,000 tokens

When to trigger compression:
- When total context > 80,000 tokens (buffer before limit)
- Transparently: user should not notice context management happening

Show: implementation in Python with token counting and selective compression.
```

---

## 🔄 Agent Workflows

### `@antigravity-workflows`
Antigravity-specific workflow patterns.
```
@antigravity-workflows Design an Antigravity agentic workflow for event content creation:
Trigger: organizer creates new event (POST /api/events)
Steps:
1. Extract key event details (title, type, date, venue)
2. Research similar successful events (web search tool)
3. Generate 3 description variants (LLM)
4. Score each variant for quality and SEO (LLM judge)
5. Select best variant
6. Generate SEO metadata (title tag, meta description, keywords)
7. Update event in database (API tool)
8. Notify organizer via Slack (Slack tool)

Error handling: if any step fails, log + continue with original description.
Show: full workflow definition using Antigravity workflow DSL.
```

### `@goal-loop` / `@ai-loop`
Agentic goal pursuit with iterative refinement.
```
@goal-loop Implement a goal-directed agent for improving our event descriptions:
Goal: all events achieve SEO quality score ≥ 80/100

Loop:
1. Fetch: get 10 events with quality score < 80
2. If none: goal achieved, stop
3. For each: analyze what's missing (too short? no keywords? no structure?)
4. Generate improved version targeting weak areas
5. Score improved version
6. If score ≥ 80: update database
7. If score < 80: retry with different approach (max 3 retries)
8. Log: track improvement rate per loop iteration
9. Go to step 1

Max iterations: 100 (prevents infinite loop)
```

### `@agentflow`
Design structured, observable agentic workflows.
```
@agentflow Create an AgentFlow for our event recommendation pipeline:
Input: user query (natural language)
Output: list of 5 recommended events with explanations

Flow:
1. ParseIntent: extract filters from query (category, date, free/paid, location)
2. SearchEvents: query database with extracted filters
3. Rank: score results by relevance to original query (semantic similarity)
4. Explain: for each top result, generate 1-sentence personalized reason
5. Format: structure as EventRecommendation[] response

Branching:
- If ParseIntent returns no filters: show popular events (default path)
- If SearchEvents returns 0 results: broaden search (remove date filter first, then category)
- If user asks follow-up: re-enter at ParseIntent with conversation context

Observable: log each node's input/output, timing, token usage.
```

---

## 🔧 Tool Building

### `@agent-tool-builder`
Build tools that agents can call.
```
@agent-tool-builder Build these tools for our EventAssistantAgent:

Tool 1: search_events
Parameters:
- query: string (natural language)
- category?: "music" | "sports" | "academic" | "cultural" | "hackathon"
- date_from?: ISO date
- date_to?: ISO date
- free_only?: boolean
- max_capacity?: integer
Returns: Event[] (top 10 matches, sorted by relevance)
Implementation: PostgreSQL full-text + pgvector hybrid search

Tool 2: register_for_event
Parameters:
- event_id: string
- user_id: string (from auth context)
Returns: Registration object | RegistrationError
Guard: check user hasn't already registered, event has capacity

Tool 3: get_ticket
Parameters:
- registration_id: string
Returns: Ticket with QR code data URL

Tool 4: join_waitlist
Parameters:
- event_id: string
- user_id: string
Returns: WaitlistEntry with position number

Show: OpenAI function calling format + Python implementation for each tool.
```

### `@mcp-builder` / `@mcp-tool-developer`
Build MCP (Model Context Protocol) servers for tool integration.
```
@mcp-builder Build an MCP server for our events platform:
This will allow any MCP-compatible AI agent to query and interact with events.

MCP Tools to expose:
1. search_events: search by keyword, category, date
2. get_event_details: full event info by ID
3. check_availability: remaining capacity + waitlist count
4. register_student: register a student for an event

MCP Resources to expose:
1. events://upcoming: list of all upcoming events (summary)
2. events://{id}: full event details

MCP Prompts:
1. find_events_for_student: pre-built prompt template for personalized search

Implementation:
- Python MCP SDK (mcp package)
- Authentication: API key in MCP server config
- Rate limiting: 100 calls/minute per client

Show: complete mcp_server.py with all tools registered and handlers implemented.
```

---

## 🎯 Skill Management

### `@skill-router`
Route incoming tasks to the right skill.
```
@skill-router Build a skill router for our agentic events system:
When an agent receives a task, it should automatically invoke the right skill.

Routing logic:
- "design the database schema" → @database-design
- "write API endpoints" → @api-design-principles + @backend-architect
- "create a React component" → @react-best-practices
- "audit for security" → @security-audit
- "write tests" → @tdd + @jest-skill
- "debug this error" → @systematic-debugging
- "review this PR" → @code-review-excellence
- Unknown → @brainstorming (default planning skill)

Implementation:
- Embedding-based routing: embed the task + embed skill descriptions
  → cosine similarity → pick closest skill
- Fallback: if confidence < 0.7, ask clarifying question
- Show top-3 skills considered with confidence scores
```

### `@skill-creator`
Create new custom skills for your specific workflow.
```
@skill-creator Create a custom skill for our events platform team:
Skill name: event-registration-code-reviewer
Purpose: review code that touches registration logic for correctness and security

SKILL.md contents:
- Persona: senior engineer who has seen every registration bug possible
- Focus areas:
  - Race conditions in capacity check + registration creation
  - ACID transaction requirements
  - Payment atomicity (no charge without record, no record without charge)
  - Idempotency (safe to retry on network failure)
  - Error messages that don't expose internals
- Review format: 
  [BLOCKING]: must fix before merge
  [CONCERN]: should discuss
  [SUGGESTION]: optional improvement
- Always: check for missing test cases covering edge cases found
```

---

## 🐛 Debugging Agents

### `@agenttrace-session-audit`
Audit a completed agent session for issues.
```
@agenttrace-session-audit Audit this agent session trace:
[paste agent session log with all tool calls, responses, reasoning]

Analyze:
- Did the agent achieve its goal efficiently?
- Were there unnecessary tool calls? (redundant searches, repeated DB queries)
- Were there reasoning errors? (wrong conclusion from correct data)
- Were there hallucinations? (claimed data not in tool results)
- Were there missed opportunities? (had the answer but didn't surface it)
- Context window waste? (including irrelevant information)

Output: session quality score + specific improvement recommendations.
```

### `@evaluation`
Systematic evaluation of agent quality.
```
@evaluation Design an evaluation framework for our EventAssistantAgent:

Evaluation dimensions:
1. Correctness: does it return accurate event information? (never hallucinate events)
2. Relevance: do recommended events match the user's intent?
3. Helpfulness: does it answer the actual question, not just a related one?
4. Safety: does it refuse inappropriate requests?
5. Efficiency: how many tool calls to achieve the goal? (less is better)

Test cases:
- Golden set: 50 queries with verified correct answers
- Adversarial: trick questions, out-of-scope requests, ambiguous queries
- Edge cases: no events match, event just sold out, user already registered

Metrics:
- Correctness: % of factual claims verified correct
- NDCG: ranking quality for event recommendations
- Tool efficiency: average tool calls per successful query
- Refusal rate: correct refusals vs over-refusals

Run evaluation: before any major model or prompt change.
```

---

## 🔗 Complete Agent Orchestration Prompt Chain

```
1️⃣  @multi-agent-architect
    "Design agent team: roles, responsibilities, communication patterns"

2️⃣  @agent-memory-systems
    "Design memory layers: working → episodic → semantic"

3️⃣  @agent-tool-builder
    "Build typed tools: search, register, validate, notify"

4️⃣  @mcp-builder
    "Package tools as MCP server for multi-agent access"

5️⃣  @agentflow
    "Define observable workflow: ParseIntent → Search → Rank → Format"

6️⃣  @dispatching-parallel-agents
    "Optimize throughput: parallel processing with semaphores"

7️⃣  @context-window-management
    "Add sliding window + summarization to stay within token limits"

8️⃣  @agenttrace-session-audit
    "Audit real session traces for inefficiency and errors"

9️⃣  @evaluation
    "Build evaluation framework: golden set, metrics, regression tests"
```
