# Project Progress Dashboard

> **For the boss**: This file is the always-current summary of the project. Read this first.
> For deeper detail: see `roadmap.md` (planned work), `ideas.md` (decisions log), and `sessions/` (per-session notes).

**Last updated**: 2026-05-15 (session 8)

---

## Current Status

**Phase**: MVP Test — SHM complete; Phase 3 items ongoing
**Overall progress**: SHM chapter fully written (8 pages), all 5 source types ingested, images extracted and embedded. Next: per-page finetuning + simulation embeds.

---

## What We're Building

An **academic knowledge base** for an AI companion that helps Thai students (primarily high schoolers preparing for O-NET/PAT/TCAS) with homework and exam prep. The companion must be highly trustworthy and pedagogically sound.

**Scope (Phase 1)**: High school math, physics, biology, chemistry — Thai national curriculum (IPST/สสวท.) as authoritative standard, supplemented by international sources.

**Architecture decision**: Hybrid — curated concept pages (LLM Wiki style) as the quality/trust layer, with RAG retrieval on top. The curation is what makes it trustworthy; RAG makes it queryable.

---

## Recent Wins

- Set up the LLM Wiki system (Claude Code as wiki maintainer)
- Made architecture decision: hybrid curated wiki + RAG
- Designed concept page format — fixed metadata shell + free-form content body; LaTeX; IPST as authority; two-phase simulation strategy
- Established `kb/physics/` with CLAUDE.md, raw/, wiki/ structure
- **SHM chapter complete** — 8 concept pages written, all 5 source types ingested:
  - `shm-definition`, `shm-equations-graphs`, `shm-spring-mass`, `shm-spring-combinations`, `shm-pendulum`, `shm-natural-frequency-resonance`, `shm-energy` (Special Topic), `shm-other-forms` (เกินหลักสูตร)
- **Tutor-style Thai standard** — language rules documented in `kb/physics/CLAUDE.md`; all pages reviewed
- **Image extraction pipeline** — PyMuPDF renders OE Textbook pages as PNG; 7 images embedded across 6 wiki pages; `wiki/assets/` established

---

## Active Focus

SHM MVP pages complete. Ready for per-page deep-dive: language finetuning + interactive simulation embeds (iframe Phase 1).

---

## Up Next

- **Per-page finetuning** — review each SHM page for language polish, clarity, example quality
- **Simulation embeds** — add iframe embed for applicable SHM pages (Phase 1)
- **Image cropping** — finalize asset crops (user handling)
- Design curriculum topic taxonomy (subject → chapter → concept hierarchy)
- Research Thai-capable embedding models
- Choose RAG layer implementation

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
| 2026-05-15 | Tutor-style Thai as permanent language standard | Formal academic register alienates students; tutor voice makes content more accessible and engaging |
| 2026-05-15 | Spring combinations → separate page | More navigable; students look up this topic independently |
| 2026-05-15 | Images from OE sources only | Copyright boundary — OE Textbook is our own company's material |

---

## Open Questions

- Which embedding model handles Thai text well? (multilingual-e5, OpenAI text-embedding-3, etc.)
- Long-term stateful simulation linking strategy — how to pass context-dependent initial state to simulations?
- Worked examples format — same page template or a different structure? (deferred)
- What is the maintenance workflow for keeping pages current as curriculum changes?
