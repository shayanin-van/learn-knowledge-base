# Roadmap

Planned work, prioritized. Updated as the project evolves.

**Last updated**: 2026-05-15

---

## Status Key

- `[ ]` Not started
- `[~]` In progress
- `[x]` Done
- `[-]` Dropped / deprioritized

---

## Phase 1 — Research & Architecture Decision

- [x] Set up LLM Wiki system
- [x] Research Karpathy LLM Wiki pattern
- [x] Research RAG and vector databases
- [x] Clarify project goals and scope
- [x] Evaluate LLM Wiki vs RAG vs hybrid for this project
- [x] Make architecture decision
- [x] Deep-dive on GraphRAG and Agentic RAG (architecture re-evaluation requested by owner)
- [x] Confirm architecture decision with evidence: hybrid curated wiki + upgraded RAG layer

## Phase 2 — Project Definition (locked behind Phase 1)

- [x] Define target audience and use cases
- [x] Define what "academic knowledge base" means for this project
- [x] Identify initial source documents / data sources

## Phase 3 — Knowledge Base Design

- [x] Design concept page format for math/science (formulas, LaTeX, step-by-step)
- [ ] Design curriculum topic taxonomy (subject → chapter → concept hierarchy)
- [ ] Research Thai-capable embedding models (for upgraded RAG layer)
- [ ] Choose and implement RAG layer upgrades: hierarchical indexing, hybrid search, query rewriting, contextual retrieval
- [ ] Define source authority and citation metadata schema
- [ ] Design simulation linking strategy

## MVP Test — Simple Harmonic Motion

Goal: produce a small, complete knowledge base for one topic to support team testing of the chat feature.

- [x] Obtain company curriculum structure for the SHM chapter (master record CSV ingested)
- [x] Establish `kb/physics/` directory with CLAUDE.md, raw/, wiki/ structure
- [x] Define source type taxonomy and ingest order
- [x] Install Poppler — PDF reading with images now working
- [x] Ingest all 5 SHM source types — S-Map, IPST Textbook, Teacher Manual, OE Textbook, International Textbook
- [x] Create concept pages for Simple Harmonic Motion — 8 pages complete
- [x] Validate and refine concept page format against real content — tutor-style language standard documented
- [x] Extract and embed OE Textbook images — 7 images in 6 pages
- [~] Finalize image crops (user handling)
- [ ] Per-page finetuning — language polish, example quality, clarity
- [ ] Add interactive simulation embeds (iframe Phase 1) to applicable SHM pages
- [ ] Confirm simulation linking approach — identify which simulations exist for SHM topics

---

## Phase 4 — Content Ingestion (locked behind Phase 3)

- [ ] Ingest IPST math textbooks
- [ ] Ingest IPST physics textbooks
- [ ] Ingest IPST biology textbooks
- [ ] Ingest IPST chemistry textbooks
- [ ] Ingest past exam papers (O-NET, PAT)
- [ ] Supplement with international sources

## Phase 3 — Build (locked behind Phase 2)

- [ ] TBD — depends on architecture decision

---

## Dropped / Deferred

*(nothing yet)*
