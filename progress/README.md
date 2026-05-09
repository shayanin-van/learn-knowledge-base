# Project Progress Dashboard

> **For the boss**: This file is the always-current summary of the project. Read this first.
> For deeper detail: see `roadmap.md` (planned work), `ideas.md` (decisions log), and `sessions/` (per-session notes).

**Last updated**: 2026-05-09 (session 2)

---

## Current Status

**Phase**: Exploration & Research (architecture re-evaluation complete)
**Overall progress**: Architecture decision confirmed with deeper evidence. GraphRAG and Agentic RAG evaluated and rejected. Hybrid curated wiki + upgraded RAG confirmed. Ready to begin Phase 3.

---

## What We're Building

An **academic knowledge base** for an AI companion that helps Thai students (primarily high schoolers preparing for O-NET/PAT/TCAS) with homework and exam prep. The companion must be highly trustworthy and pedagogically sound.

**Scope (Phase 1)**: High school math, physics, biology, chemistry — Thai national curriculum (IPST/สสวท.) as authoritative standard, supplemented by international sources.

**Architecture decision**: Hybrid — curated concept pages (LLM Wiki style) as the quality/trust layer, with RAG retrieval on top. The curation is what makes it trustworthy; RAG makes it queryable.

---

## Recent Wins

- Set up the LLM Wiki system (Claude Code as wiki maintainer)
- Bootstrapped wiki with foundational knowledge on LLM Wiki, RAG, and vector databases
- Clarified full project scope, audience, and constraints in session 2026-05-09
- Made the architecture decision: hybrid curated wiki + RAG
- Created `project-context.md` and `knowledge-base-architecture.md` wiki pages
- Deep-dive on GraphRAG and Agentic RAG (4 sources ingested + web research)
- Confirmed architecture decision with stronger evidence; identified specific RAG layer upgrades
- Created `graphrag.md`, `agentic-rag.md`, `rag-architecture-comparison.md` wiki pages

---

## Active Focus

- Architecture decision confirmed. Ready to begin Phase 3 (Knowledge Base Design).

---

## Up Next

- Design concept page format for math/science content (formulas, LaTeX, step-by-step)
- Research Thai-capable embedding models for the upgraded RAG layer
- Choose RAG layer implementation: hierarchical indexing, hybrid search, query rewriting, contextual retrieval
- Begin building the concept page taxonomy (curriculum topic hierarchy)
- Add first source documents to `raw/` (IPST textbooks, past exam papers)

---

## Key Decisions Made

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-05-08 | Using LLM Wiki pattern for this research wiki | Karpathy's approach fits a curated, exploratory knowledge base well |
| 2026-05-08 | Progress tracking system established in `progress/` | Needed a boss-readable record and future chatbot data source |
| 2026-05-09 | Hybrid architecture: curated wiki + RAG | Pure RAG too unreliable for high-stakes student content; pure wiki too static for diverse sources. Quality comes from curation; RAG makes it queryable. |
| 2026-05-09 | IPST/สสวท. as authoritative source; international as enrichment | IPST is the Thai national curriculum standard; must win when sources conflict |
| 2026-05-09 (s2) | GraphRAG rejected | Designed for unstructured narrative data; underperforms standard RAG on math textbooks; brittle to updates; our wiki already IS a manual knowledge graph |
| 2026-05-09 (s2) | Full Agentic RAG rejected | Compound error risk (95%/step → 60% over 10 steps); over-complex for direct student queries; high-stakes context requires predictability |
| 2026-05-09 (s2) | RAG layer to adopt 4 Agentic RAG techniques | Hierarchical indexing, hybrid search (dense+BM25+RRF), query rewriting, contextual retrieval — proven improvements without agent complexity |

---

## Open Questions

- Which embedding model handles Thai text well? (multilingual-e5, OpenAI text-embedding-3, etc.)
- How to represent math content in concept pages — LaTeX? MathML? Plain Thai descriptions?
- How will interactive simulations be linked to concept pages?
- What is the maintenance workflow for keeping pages current as curriculum changes?
