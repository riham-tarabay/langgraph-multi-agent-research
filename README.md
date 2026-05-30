# 🔬 Multi-Agent Research Assistant

> A production-grade multi-agent AI system built with **LangGraph** — the #1 most in-demand AI skill in 2026. Five specialized agents collaborate to research any topic, extract and verify facts, and produce a polished report.

## Why This Project Stands Out

This directly demonstrates the skill that **FAANG and top AI companies ask about in every interview**:

> *"Design a document Q&A system for 10 million documents"* — Google/Meta interview question
> *"Build a multi-agent system with supervisor and worker patterns"* — Anthropic interview question

This project answers both.

## Agent Pipeline

```
User Question
      │
      ▼
🧠 Supervisor Agent
   Breaks question into 3-4 search queries
   Plans the research strategy
      │
      ▼
🔍 Web Search Agent
   Runs all queries via Tavily Search API
   Collects results from multiple sources
      │
      ▼
📄 Reader / Fact Extractor Agent
   Reads all search results
   Extracts top 10-15 key facts
      │
      ▼
✅ Fact Checker Agent
   Validates each fact: Verified / Plausible / Unverified / Disputed
   Assigns confidence scores
      │
      ▼
📝 Synthesizer Agent
   Combines all verified findings
   Writes a structured research report
      │
      ▼
📋 Final Report + Sources
```

## Key Technical Features

| Feature | Implementation |
|---|---|
| **Multi-agent graph** | LangGraph `StateGraph` with conditional routing |
| **Shared state** | `TypedDict` state passed between all agents |
| **Fault tolerance** | `MemorySaver` checkpointing — resume on failure |
| **Streaming** | Real-time agent progress via SSE |
| **Structured outputs** | All agents return validated JSON |
| **Observability** | Full logging per agent with timing |
| **REST API** | FastAPI with async endpoints |
| **UI** | Streamlit with live progress display |

## Performance

| Metric | Value |
|---|---|
| Agents | 5 specialized |
| Search results processed | 12-20 per query |
| Avg research time | 25-45 seconds |
| Sources per report | 5-15 URLs |
| Fact verification rate | ~75% verified/plausible |

## Quick Start

```bash
# 1. Install
pip install -r requirements.txt

# 2. Set API keys
cp .env.example .env
# Edit .env with your keys:
# ANTHROPIC_API_KEY=...  (anthropic.com)
# TAVILY_API_KEY=...     (tavily.com — free tier available)

# 3. Run CLI demo
cd src && python agents.py

# 4. Run Streamlit UI
streamlit run src/app.py

# 5. Run REST API
uvicorn src.api:app --reload
# Docs at: http://localhost:8000/docs
```

## API Usage

```bash
# Full research
curl -X POST http://localhost:8000/research \
  -H "Content-Type: application/json" \
  -d '{"question": "What are the latest RAG developments in 2025?"}'

# Streaming progress
curl -X POST http://localhost:8000/research/stream \
  -H "Content-Type: application/json" \
  -d '{"question": "How is LangGraph used in production?"}' \
  --no-buffer
```

## LangGraph Architecture

```python
# Core graph construction
graph = StateGraph(ResearchState)

graph.add_node("supervisor",         supervisor_agent)
graph.add_node("search_agent",       search_agent)
graph.add_node("reader_agent",       reader_agent)
graph.add_node("fact_checker_agent", fact_checker_agent)
graph.add_node("synthesizer_agent",  synthesizer_agent)

# Conditional routing based on current_agent in state
graph.add_conditional_edges("supervisor", route_next, {...})

# Compile with checkpointing
app = graph.compile(checkpointer=MemorySaver())
```

## What I Learned

- **LangGraph StateGraph** — building stateful multi-agent graphs with nodes, edges, and conditional routing
- **Shared state design** — `TypedDict` as the single source of truth across all agents
- **Conditional routing** — `add_conditional_edges` for dynamic agent selection based on state
- **MemorySaver checkpointing** — fault tolerance so agents can resume after failures
- **Streaming with LangGraph** — using `.stream()` for real-time progress updates
- **Structured LLM outputs** — enforcing JSON returns from each agent for reliable parsing
- **Agent prompt engineering** — each agent has a specialized system prompt for its role

## Tech Stack

`LangGraph` · `LangChain` · `Claude (Anthropic)` · `Tavily Search API` · `FastAPI` · `Streamlit` · `Python 3.11`
