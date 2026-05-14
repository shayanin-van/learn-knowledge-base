# Wiki Index

Table of contents for the knowledge base. Updated after every change.

**Last updated**: 2026-05-14 (session 4)

---

## Project

| Page | Description |
|---|---|
| [[project-context]] | Company, target users, goals, and scope of the academic knowledge base project |
| [[knowledge-base-architecture]] | Chosen architecture: hybrid curated wiki layer + RAG retrieval, and the rationale |

## Knowledge Base Design

| Page | Description |
|---|---|
| [[concept-page-format]] | Standard format for concept pages — fixed metadata shell, free-form content body, LaTeX, simulation linking |

## Knowledge Base Approaches

| Page | Description |
|---|---|
| [[karpathy-llm-wiki]] | Karpathy's pattern: LLM as compiler, building a compounding Markdown wiki from raw sources |
| [[rag]] | Retrieval-Augmented Generation — architecture for grounding LLM responses in external documents |
| [[vector-database]] | Specialized database for semantic similarity search; the retrieval engine inside RAG systems |
| [[graphrag]] | Microsoft's knowledge-graph-based RAG — how it works, where it excels (unstructured discovery), where it fails (structured curriculum content) |
| [[agentic-rag]] | Agent-orchestrated multi-step retrieval — key techniques (hierarchical indexing, hybrid search, query rewriting) and what to borrow vs. avoid |
| [[rag-architecture-comparison]] | Side-by-side analysis of Standard RAG, GraphRAG, Agentic RAG, and Hybrid Wiki+RAG against the Thai academic KB constraints |
| [[rag-pipeline-implementation]] | Practical RAG pipeline — text extraction, splitting strategies, embedding model trade-offs, vector database options, generation component |
| [[advanced-rag-techniques]] | Production-grade improvements: chunk decoupling, Lost in the Middle problem, hybrid retrieval merging strategies, query routing, user history caching |
