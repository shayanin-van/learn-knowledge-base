# Project Progress Dashboard

> **For the boss**: This file is the always-current summary of the project. Read this first.
> For deeper detail: see `roadmap.md` (planned work), `ideas.md` (decisions log), and `sessions/` (per-session notes).

**Last updated**: 2026-05-09

---

## Current Status

**Phase**: Exploration & Research
**Overall progress**: Project scope defined; architecture decision made. Ready to begin ingesting source materials.

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

---

## Active Focus

- Ready to ingest source documents (textbooks, exam papers) into `raw/`

---

## Up Next

- Add first source documents to `raw/` (IPST textbooks, past exam papers)
- Design the concept page format for math/science content (formulas, LaTeX, step-by-step)
- Explore Thai-capable embedding models for the vector index layer
- Begin building the concept page taxonomy (curriculum topic hierarchy)

---

## Key Decisions Made

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-05-08 | Using LLM Wiki pattern for this research wiki | Karpathy's approach fits a curated, exploratory knowledge base well |
| 2026-05-08 | Progress tracking system established in `progress/` | Needed a boss-readable record and future chatbot data source |
| 2026-05-09 | Hybrid architecture: curated wiki + RAG | Pure RAG too unreliable for high-stakes student content; pure wiki too static for diverse sources. Quality comes from curation; RAG makes it queryable. |
| 2026-05-09 | IPST/สสวท. as authoritative source; international as enrichment | IPST is the Thai national curriculum standard; must win when sources conflict |

---

## Open Questions

- Which embedding model handles Thai text well? (multilingual-e5, OpenAI text-embedding-3, etc.)
- How to represent math content in concept pages — LaTeX? MathML? Plain Thai descriptions?
- How will interactive simulations be linked to concept pages?
- What is the maintenance workflow for keeping pages current as curriculum changes?
