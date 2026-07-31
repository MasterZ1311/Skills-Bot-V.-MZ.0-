# 🤖 AI / ML Engineer — Skills Guide

AI/ML engineers build intelligent features — from RAG systems and LLM integrations to fine-tuned models, MLOps pipelines, and voice/vision AI. This guide covers the full AI engineering stack.

---

## 🗺️ Skill Map

| Concern | Top Skills |
|---|---|
| LLM Apps | `@llm-app-patterns`, `@prompt-engineering`, `@llm-structured-output` |
| Frameworks | `@langchain-architecture`, `@langgraph`, `@crewai`, `@pydantic-ai` |
| RAG | `@rag-engineer`, `@rag-implementation`, `@embedding-strategies`, `@weaviate` |
| HuggingFace | `@huggingface-best`, `@hugging-face-model-trainer`, `@trl-training` |
| Google AI | `@gemini-api-dev`, `@gemini-live-api-dev`, `@ai-studio-image` |
| MLOps | `@mlops-engineer`, `@ml-pipeline-workflow`, `@langfuse` |
| Voice AI | `@voice-agents`, `@voice-ai-development`, `@azure-ai-voicelive-py` |
| Image AI | `@fal-generate`, `@stability-ai`, `@comfyui-gateway` |
| Agents | `@autonomous-agents`, `@agent-creator`, `@multi-agent-architect` |

---

## 🧠 LLM Application Development

### `@llm-app-patterns`
Architectural patterns for LLM-based applications.
```
@llm-app-patterns Design the AI assistant for our events platform:
Feature: "Ask about events" — students type natural language queries
Patterns to consider:
- RAG: retrieve relevant events → generate answer
- Tool use: structured search function + LLM reasoning
- Streaming: stream response as tokens arrive
- Caching: cache embeddings and frequent queries
- Fallback: if LLM fails, return keyword search results
```

### `@prompt-engineering` / `@llm-prompt-optimizer` / `@prompt-engineering-patterns`
Crafting effective prompts and system instructions.
```
@prompt-engineering Design the system prompt for our events assistant:
- Persona: helpful campus events guide
- Constraints: only discuss campus events, dates, registration
- Format: respond in 2-3 sentences max, use bullet points for multiple events
- Edge cases: out-of-scope questions, unavailable events
- Anti-patterns to avoid: hallucinating event details

Then optimize this user prompt:
"find music things happening" → improve for better retrieval
```

### `@llm-structured-output`
Getting reliable, parseable outputs from LLMs.
```
@llm-structured-output Implement structured output for event extraction:
Given a natural language event description, extract:
{
  title: string,
  date: ISO 8601,
  venue: string,
  category: "music" | "sports" | "academic" | "cultural",
  requiresRegistration: boolean,
  price: number | null
}
Use: Pydantic (Python) or Zod (TypeScript) + instructor/LangChain structured output.
```

---

## 🦜 LLM Frameworks

### `@langchain-architecture` 
LangChain chains, agents, tools, callbacks.
```
@langchain-architecture Build a LangChain RAG chain for events Q&A:
- Retriever: Chroma/Weaviate with event embeddings
- LLM: OpenAI GPT-4o (or Gemini)
- Memory: ConversationBufferWindowMemory (last 5 turns)
- Chain: ConversationalRetrievalChain
- Callbacks: LangSmith tracing
- Streaming: AsyncIterator for token streaming
```

### `@langgraph`
LangGraph for stateful, multi-step AI workflows.
```
@langgraph Build a LangGraph workflow for event recommendations:
State: { userPreferences, searchResults, refinements, finalRecommendations }
Nodes:
1. analyzePreferences: extract interests from user message
2. searchEvents: query database with extracted filters
3. rankResults: use LLM to rank by relevance
4. checkSatisfaction: are results good? If not → refineSearch
5. formatResponse: structure final answer
Add conditional edge: if results < 3 → refineSearch, else → formatResponse
```

### `@crewai`
Multi-agent teams with CrewAI.
```
@crewai Build a CrewAI team for event content generation:
Agents:
- EventResearcher: finds similar successful events online
- ContentWriter: writes compelling event description
- SEOOptimizer: adds keywords and meta description
- QualityReviewer: checks accuracy and tone

Task: generate complete event listing for "Annual Coding Hackathon"
```

### `@pydantic-ai`
Type-safe AI development with Pydantic AI.
```
@pydantic-ai Build a Pydantic AI agent for event registration assistance:
- Model: OpenAI GPT-4o
- Tools: search_events(query), check_availability(event_id), get_user_registrations()
- Structured output: RecommendationResponse with events list + reasoning
- Type safety: full Pydantic validation on inputs and outputs
```

---

## 📚 RAG Systems

### `@rag-engineer` / `@rag-implementation`
Complete RAG pipeline design and implementation.
```
@rag-engineer Build a RAG system for our events platform:

Indexing pipeline:
1. Fetch all events from database
2. Chunk: title + description + tags (no chunking needed, small docs)
3. Embed: text-embedding-3-small (OpenAI) or all-MiniLM-L6-v2 (free)
4. Store: Weaviate (production) or ChromaDB (local dev)
5. Metadata: event_id, date, category, capacity (for filtered search)

Query pipeline:
1. Embed user query
2. Hybrid search: semantic + keyword
3. Filter: only future events
4. Rerank: top 10 → top 3 with cross-encoder
5. Generate: GPT-4o with retrieved context
```

### `@embedding-strategies`
Choosing and implementing embedding models.
```
@embedding-strategies Select the right embedding strategy for our events:
Options:
- OpenAI text-embedding-3-small: best quality, $0.02/1M tokens
- sentence-transformers/all-MiniLM-L6-v2: free, good for English
- multilingual-e5-large: if we need Hindi/regional language support

For our 10,000 events: cost estimate, quality tradeoff, latency.
Show implementation in Python for batch embedding + upsert to pgvector.
```

### `@hybrid-search-implementation`
Combine keyword and vector search for best results.
```
@hybrid-search-implementation Implement hybrid search in PostgreSQL + pgvector:
1. Full-text search: tsvector on title + description
2. Vector search: pgvector cosine similarity on embedding column
3. Combine: Reciprocal Rank Fusion
   - keyword_rank, vector_rank → fused_score
4. Filter: AND (date > NOW() AND category = 'music')
5. Return: top 5 with scores
Show the complete SQL query.
```

---

## 🤗 HuggingFace Skills

### `@huggingface-best`
Best practices for using HuggingFace models and APIs.
```
@huggingface-best We want to use HuggingFace for:
1. Text classification: categorize event descriptions
2. Named entity recognition: extract date/venue from announcements
3. Image classification: detect event category from uploaded flyers

Recommend: Inference API vs local model vs fine-tuning.
For each: model choice, cost estimate, latency, accuracy expectation.
```

### `@hugging-face-model-trainer` / `@trl-training`
Fine-tuning models on custom data.
```
@hugging-face-model-trainer Fine-tune a classification model for our events:
Categories: academic, cultural, sports, music, hackathon, workshop
Training data: 500 labeled events descriptions
Base model: distilbert-base-uncased (lightweight)
Steps:
1. Prepare dataset in HuggingFace datasets format
2. Tokenizer setup
3. Training with Trainer API
4. Evaluation: F1 score per class
5. Push to HuggingFace Hub
6. Inference endpoint or local deployment
```

### `@huggingface-spaces`
Deploy models to HuggingFace Spaces.
```
@huggingface-spaces Deploy our event classifier as a HuggingFace Space:
- Gradio interface for testing
- Model: our fine-tuned distilbert
- Input: event description text area
- Output: category + confidence score
- Shareable with the team for testing
```

---

## 🌟 Google Gemini

### `@gemini-api-dev` / `@gemini-api-integration`
Integrating Google Gemini into applications.
```
@gemini-api-dev Integrate Gemini into our events platform:
- gemini-2.0-flash for event Q&A (fast, cheap)
- gemini-2.5-pro for complex event planning advice
- Streaming responses with Server-Sent Events
- Gemini's function calling for tool use
- Safety settings: block harmful content
- Context caching for system prompt (cost savings)
```

### `@gemini-live-api-dev`
Real-time voice/video with Gemini Live.
```
@gemini-live-api-dev Build a voice assistant for event queries using Gemini Live:
- WebSocket connection to Gemini Live API
- Send audio from browser microphone
- Receive text + audio response
- Display transcript
- Use case: "Hey, what events are happening this weekend?"
```

---

## 🔊 Voice AI

### `@voice-agents` / `@voice-ai-development`
Building voice-enabled AI applications.
```
@voice-ai-development Build a voice check-in system for events:
- Student says their name at the entrance
- ASR: transcribe speech to text
- Entity extraction: find name in our attendee list
- TTS: "Welcome, Siva! Enjoy the hackathon!"
- Fallback: QR scan if voice fails
Stack: Deepgram (ASR) + GPT-4o (reasoning) + ElevenLabs (TTS)
```

---

## 🖼️ Image AI

### `@fal-generate` / `@fal-image-edit`
AI image generation with fal.ai.
```
@fal-generate Generate event banner images automatically:
When admin creates an event:
- Extract: event title, category, mood
- Generate prompt: "Professional event banner for [title], 
  [category] theme, vibrant colors, campus setting, 16:9 ratio"
- Use fal.ai Flux model
- Save to our storage and attach to event
```

### `@stability-ai`
Stable Diffusion for image generation.
```
@stability-ai Create a batch image generation pipeline:
Input: 50 events without images
For each: generate relevant banner using Stable Diffusion 3.5
Style: consistent college events aesthetic, warm colors, 
Include: event category in prompt (music has instruments, sports has athletes)
Batch: process 10 at a time to avoid rate limits
```

---

## 🚀 MLOps

### `@mlops-engineer` / `@ml-pipeline-workflow`
Production ML pipeline design.
```
@mlops-engineer Design the ML pipeline for our event recommendation system:
Training:
- Data: user registration history, event attributes
- Model: collaborative filtering + content-based hybrid
- Training: weekly retrain on new data
- Evaluation: A/B test against non-ML baseline

Serving:
- Real-time: precomputed recommendations stored in Redis
- Freshness: recompute when new event added or user registers
- Fallback: popularity-based recommendations
Monitoring: recommendation click-through rate, diversity score
```

### `@langfuse`
LLM observability and tracing.
```
@langfuse Instrument our events AI assistant with Langfuse:
- Trace every query end-to-end: input → retrieval → LLM → output
- Track: latency per step, token count, cost per query
- User feedback: thumbs up/down on recommendations
- A/B test: GPT-4o vs Gemini Flash (track quality metrics)
- Dashboard: daily active users, average query cost, error rate
```

---

## 🔗 Complete AI/ML Prompt Chain

```
1️⃣  @rag-engineer
    "Design RAG pipeline: indexing events, hybrid search, reranking"

2️⃣  @embedding-strategies
    "Select and implement embedding model: batch embed all events"

3️⃣  @weaviate (or pgvector)
    "Set up vector store, index events, test similarity search"

4️⃣  @langchain-architecture (or @langgraph)
    "Build conversational RAG chain with memory"

5️⃣  @prompt-engineering
    "Design and optimize system prompt and query templates"

6️⃣  @llm-structured-output
    "Add structured output for filtered search queries"

7️⃣  @gemini-api-dev (or OpenAI)
    "Integrate LLM with streaming and function calling"

8️⃣  @langfuse
    "Add observability: trace every query, track costs and quality"

9️⃣  @mlops-engineer
    "Production pipeline: monitoring, retraining, A/B testing"
```
