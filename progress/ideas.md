# Ideas & Decisions Log

Running record of ideas explored and decisions made. Append-only — never delete entries.

**Last updated**: 2026-05-08

---

## 2026-05-08

### Idea: Progress tracking as a future chatbot data source
The progress tracking system should be structured and machine-readable — not just human notes — so it can eventually feed a chatbot that briefs the boss on project status autonomously.

**Status**: Adopted — this directory is designed with that in mind.

---

### Idea: Use Karpathy's LLM Wiki pattern for this research wiki
Instead of a RAG system, use Claude Code as a wiki maintainer: raw sources stay immutable, Claude writes and maintains structured Markdown pages with cross-links, and knowledge compounds over time.

**Status**: Adopted — the `wiki/` directory follows this pattern.

---

### Decision: Hybrid architecture — curated wiki layer + RAG retrieval
After discussing project scope, the architecture for the actual knowledge base is: curated concept pages (LLM Wiki style) as the trustworthiness layer, with RAG vector retrieval on top. Pure RAG over raw PDFs is too unreliable for high-stakes student-facing content. Pure wiki is too static for diverse Thai + international sources at curriculum scale.

Key principle: **quality comes from curation, not retrieval**. RAG operates on curated pages, not raw source documents.

**Status**: Decided — 2026-05-09. See `wiki/knowledge-base-architecture.md`.

---

### Idea: Interactive simulations as a knowledge layer
The project owner plans to build coded interactive simulations (e.g., physics visualizations) that can be linked to concept pages. These are a distinct knowledge type — not text — and will need a separate linking strategy in the knowledge base.

**Status**: Deferred — noted for Phase 3 design. Will revisit when defining the concept page format.

---

### Open question: LLM Wiki vs RAG vs hybrid for the actual project
The wiki uses the LLM Wiki pattern for *this* research. But the *project being researched* (an academic knowledge base) may need a different architecture depending on scale, audience, and update frequency.

**Status**: Resolved 2026-05-09 — hybrid chosen. See above.

---

## 2026-05-09 (session 2)

### Analysis: GraphRAG not suitable for this project
Deep-dive on Microsoft's GraphRAG (knowledge graph RAG) against our use case. Key findings:
- GraphRAG is designed for unstructured narrative data (news articles, social media) — not structured curriculum textbooks
- arXiv:2509.16780 (2025): undergraduate math textbook, 477 QA pairs — standard RAG outperforms GraphRAG on retrieval accuracy (Top 1/3/5/10) and F1 score. GraphRAG over-retrieves due to entity-based traversal.
- GraphRAG is brittle to curriculum updates (re-indexing large graph portions required)
- Our curated wiki with wiki-links IS a knowledge graph — manually curated with human accuracy, not LLM auto-extraction noise

**Status**: Decided — GraphRAG rejected. See `wiki/graphrag.md` and `wiki/rag-architecture-comparison.md`.

---

### Analysis: Full Agentic RAG not suitable, but component techniques are valuable
Deep-dive on Agentic RAG (LLM agent orchestrating multi-step retrieval). Key findings:
- Full agent loop introduces compound error risk (95% accuracy/step → 60% success over 10 steps — Huyen). High-stakes exam prep amplifies this risk.
- Most student queries are direct; agent complexity adds latency and unpredictability with no benefit
- However, specific component techniques are proven and worth adopting: hierarchical indexing, hybrid search (dense + BM25 + RRF), query rewriting, contextual retrieval (Anthropic's chunk-with-context approach)

**Status**: Decided — full Agentic RAG rejected; component techniques to be adopted in Phase 3 RAG layer design. See `wiki/agentic-rag.md`.

---

### Decision confirmed: Hybrid curated wiki + upgraded RAG
Architecture re-evaluation complete. The original architecture decision (hybrid curated wiki + RAG) is confirmed, now with stronger evidence and a more specific RAG implementation plan.

The upgrade: RAG layer to use hierarchical indexing, hybrid search (dense + sparse with RRF), query rewriting, and contextual retrieval — all adopted from Agentic RAG research, without the agent orchestration overhead.

**Status**: Confirmed 2026-05-09 (session 2). See `wiki/rag-architecture-comparison.md`.
