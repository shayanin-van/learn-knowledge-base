# Project Progress Dashboard

> **For the boss**: This file is the always-current summary of the project. Read this first.
> For deeper detail: see `roadmap.md` (planned work), `ideas.md` (decisions log), and `sessions/` (per-session notes).

**Last updated**: 2026-05-15 (session 5)

---

## Current Status

**Phase**: Phase 3 — Knowledge Base Design (in progress)
**Overall progress**: Concept page format designed and documented. One Phase 3 item complete; five remaining.

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
- Ingested practical RAG ebook (5 chapters); architecture validated against literature
- Created `rag-pipeline-implementation.md` and `advanced-rag-techniques.md` wiki pages
- Identified "Lost in the Middle" as a concrete production risk; added to architecture notes
- **Designed concept page format** — fixed metadata shell + free-form content body; LaTeX standard; parameterized simulation linking; IPST as conflict-resolution authority only
- **Updated simulation strategy** — two-phase: iframe for MVP testing, custom `simulation` JSON block for production
- **Established `kb/` directory** — the actual academic KB layer, separate from research wiki; subject-level folder structure

---

## Active Focus

MVP Test — SHM. `kb/physics/` fully set up with source taxonomy, master record documented, Poppler installed. Next session: ingest all 5 SHM sources in order, then write concept pages.

---

## Up Next

- Design curriculum topic taxonomy (subject → chapter → concept hierarchy)
- Research Thai-capable embedding models for the upgraded RAG layer
- Choose RAG layer implementation: hierarchical indexing, hybrid search, query rewriting, contextual retrieval
- Define source authority and citation metadata schema
- Design simulation linking strategy (long-term stateful approach)
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
| 2026-05-14 | Concept page format finalized | Free-form content body (not rigid sections); LaTeX standard; IPST as conflict-resolution only; prerequisites as wiki-links; parameterized simulation URLs |

---

## Open Questions

- Which embedding model handles Thai text well? (multilingual-e5, OpenAI text-embedding-3, etc.)
- Long-term stateful simulation linking strategy — how to pass context-dependent initial state to simulations?
- Worked examples format — same page template or a different structure? (deferred)
- What is the maintenance workflow for keeping pages current as curriculum changes?
