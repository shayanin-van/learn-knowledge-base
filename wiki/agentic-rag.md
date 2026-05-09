# Agentic RAG

**Summary**: A RAG architecture where an LLM agent dynamically plans and iterates multi-step retrieval, rather than executing a single fixed retrieve-then-generate pipeline.

**Sources**: `raw/A modular Agentic RAG built with LangGraph — learn Retrieval-Augmented Generation Agents in minutes..md`, `raw/RAG and Agent - Snippet from the book - AI Engineering by Huyen.pdf`

**Last updated**: 2026-05-09

---

## What It Is

Standard RAG is a fixed pipeline: query → retrieve → generate. Agentic RAG replaces that fixed pipeline with an LLM agent that can decide which retrieval steps to take, in what order, and whether to retry or refine before generating an answer.

The agent treats retrieval as a tool call (source: Huyen). Because RAG is a special case of an agent — where the retriever is simply one tool among many — this upgrade is a natural extension, not a different paradigm.

## Key Components

### Hierarchical Indexing
Documents are indexed at two levels simultaneously (source: LangGraph repo):
- **Parent chunks**: large, structured by Markdown headers or logical sections
- **Child chunks**: small, fixed-size (e.g., 200–400 tokens), used for precise embedding search

At query time, search runs on child chunks for high-precision retrieval, but the full parent chunk is returned for context. This solves the chunking dilemma: small chunks are good for finding the right passage, large chunks are good for answering from it.

### Query Rewriting
Before retrieval, the agent rewrites the user's query to improve embedding search quality. A raw question like "what happens to force when mass doubles?" is rewritten into a more retrieval-friendly form (source: Huyen). This is especially valuable when student queries are colloquial or ambiguous.

### Hybrid Search
Combines dense retrieval (embedding similarity) with sparse retrieval (BM25/keyword matching). Results from both are merged using Reciprocal Rank Fusion (RRF) (source: LangGraph repo). This catches cases where exact terms matter (e.g., a specific formula name) and cases where semantic meaning matters more.

### Contextual Retrieval
Before indexing, each chunk is augmented with a short (50–100 token) AI-generated context that explains where the chunk fits in the larger document (source: Huyen / Anthropic). This dramatically improves retrieval accuracy for chunks that would otherwise be too short to carry enough meaning.

### Context Compression
Before passing retrieved chunks to the LLM, the agent compresses them — removing irrelevant sentences and keeping only the parts most relevant to the query. This keeps the context window focused (source: LangGraph repo).

### Orchestrator with Reflection and Self-Correction
The agent runs a loop: plan → retrieve → evaluate → re-retrieve if needed → generate. It can decide whether the retrieved content is sufficient or whether to reformulate the query and try again (source: LangGraph repo). The LangGraph implementation uses `MAX_TOOL_CALLS=8` and `MAX_ITERATIONS=10` as safety caps.

### Multi-Agent Map-Reduce
For complex queries spanning many documents, multiple sub-agents retrieve in parallel, then an aggregator merges their outputs into a single answer (source: LangGraph repo).

## Strengths

- Handles multi-hop questions that require combining information from multiple sources
- Query rewriting and hybrid search improve retrieval quality over standard RAG
- Contextual retrieval and hierarchical indexing reduce chunking errors
- Self-correction loops reduce hallucination on difficult queries
- Flexible: agent can call any tool (web search, calculator, etc.), not just a vector index

## Weaknesses

- **Compound error risk**: Each agent step has failure probability. At 95% per step, 10 steps yields 60% success; 100 steps yields 0.6% (source: Huyen). For high-stakes contexts, compounding is dangerous.
- **Latency**: Multi-step retrieval loops add significant latency compared to a single-pass retrieval pipeline.
- **Unpredictability**: Agent behavior is harder to audit and test than a fixed pipeline. Debugging a failure is more complex.
- **Overkill for simple queries**: Most student questions ("what is Newton's second law?") don't need multi-hop reasoning. Running an agent loop adds cost and complexity with no benefit.
- **Requires careful guardrails**: Without safety caps and well-designed prompts, agents can loop, hallucinate retrieval decisions, or retrieve irrelevant content.

## What to Borrow vs. What to Avoid

The full agentic loop (orchestrator, reflection, multi-agent map-reduce) adds complexity that is inappropriate for most student queries. However, several component techniques are highly valuable and can be adopted independently:

| Technique | Worth adopting? | Reason |
|---|---|---|
| Hierarchical indexing | Yes | Solves chunking precision/context tradeoff cleanly |
| Query rewriting | Yes | Student queries are often colloquial; rewriting improves retrieval |
| Hybrid search | Yes | Catches formula names AND semantic concepts |
| Contextual retrieval | Yes | Improves embedding quality for short chunks |
| Context compression | Maybe | Useful if context window pressure becomes an issue |
| Full orchestrator loop | No | Compound error risk; most queries don't need multi-hop |
| Multi-agent map-reduce | No | Over-engineered for curriculum-scoped queries |

## Related Pages

- [[rag]]
- [[graphrag]]
- [[vector-database]]
- [[rag-architecture-comparison]]
- [[knowledge-base-architecture]]
