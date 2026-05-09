# Wiki Log

Append-only record of all operations. Most recent entry at the top.

---

## 2026-05-09 — Project context and architecture decision

**Source**: Discussion with project owner

**Pages created**:
- `project-context.md` — Company background, target users (Thai high school students), AI companion goals, language (Thai/IPST-authoritative), content scope (math + 3 sciences), source material, pedagogy requirements
- `knowledge-base-architecture.md` — Hybrid architecture decision: curated concept pages (LLM Wiki style) as the quality layer, with RAG retrieval on top; rationale for rejecting pure RAG and pure LLM Wiki; source authority hierarchy (IPST > international); open questions

**Pages updated**:
- `index.md` — Added "Project" section with two new pages

---

## 2026-05-08 — Initial bootstrap (web research)

**Source**: Web research (no raw/ files yet)

**Pages created**:
- `karpathy-llm-wiki.md` — Karpathy's LLM Wiki pattern: core idea, architecture, comparison to RAG, linting, Memex inspiration
- `rag.md` — RAG architecture: pipeline, benefits, limitations, comparison to LLM Wiki, 2025 trends
- `vector-database.md` — Vector databases: how they work, comparison table of common options, role in RAG, limitations
- `index.md` — Table of contents (new)
- `log.md` — This file (new)

**Reason**: User requested bootstrapping the wiki with background knowledge before ingesting project-specific source documents.
