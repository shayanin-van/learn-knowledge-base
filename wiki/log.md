# Wiki Log

Append-only record of all operations. Most recent entry at the top.

---

## 2026-05-12 (session 3) — Practical RAG implementation book ingested

**Source ingested**:
- `raw/A Practical Approach to Retrieval Augmented Generation Systems/` — 5-chapter ebook (Allahyari & Yang): LLM background, RAG fundamentals, pipeline implementation, advanced RAG techniques, observability tools

**Pages created**:
- `rag-pipeline-implementation.md` — Two-stage pipeline (ingestion + query), text splitting trade-offs (character vs token), chunk size guidance, embedding model comparison and MTEB benchmark reference, vector database options, generation component patterns
- `advanced-rag-techniques.md` — Chunk decoupling (Document Summary Index, Sentence Window), Lost in the Middle problem and reordering fix, hybrid retrieval merging strategies (RRF, concatenation, score-based), query rewriting (multilingual expansion, sub-queries), query routing, user history caching

**Pages updated**:
- `rag.md` — Added "Lost in the Middle" and "Chunk size is non-trivial" to limitations; added links to new pages
- `knowledge-base-architecture.md` — Added "Additional Architectural Notes" section: LotM mitigation, hybrid retrieval validation, two-layer architecture formally validated, user history caching note; updated open question on embedding models to reference MTEB
- `index.md` — Added two new pages

**Key findings**: Our hybrid two-layer architecture independently matches the "Document Summary Index" (Dense Hierarchical Retrieval) technique from the literature — this is a validation. Lost in the Middle is a concrete production risk requiring document reordering. Hybrid retrieval (BM25 + semantic) is specifically recommended for niche-domain terminology, directly applicable to Thai academic content. Chunk size evaluation was flagged as important but deferred — outside current time constraints.

---

## 2026-05-09 (session 2) — Architecture deep-dive: GraphRAG and Agentic RAG

**Sources ingested**:
- `GraphRAG_ A new approach for discovery using complex information.md` — Microsoft Research Blog (Feb 2024)
- `GraphRAG_Microsoft introduction.md` — Official GraphRAG docs
- `A modular Agentic RAG built with LangGraph — learn Retrieval-Augmented Generation Agents in minutes..md` — LangGraph implementation walkthrough
- `RAG and Agent - Snippet from the book - AI Engineering by Huyen.pdf` — Chapters on RAG, agents, memory (Chip Huyen)

**Pages created**:
- `graphrag.md` — GraphRAG: how it works (entity extraction, Leiden clustering, community summaries, query modes), where it excels (unstructured multi-source discovery), where it fails (structured curriculum content), evidence from math textbook evaluation, comparison to our manual wiki
- `agentic-rag.md` — Agentic RAG: key components (hierarchical indexing, query rewriting, hybrid search, contextual retrieval, orchestrator loop), strengths and weaknesses, compound error risk, what to borrow vs. avoid
- `rag-architecture-comparison.md` — Evaluation scorecard comparing Standard RAG, GraphRAG, Agentic RAG, and Hybrid Wiki+RAG against project constraints; recommendation with rationale

**Pages updated**:
- `index.md` — Added three new pages under "Knowledge Base Approaches"

**Key finding**: GraphRAG is not suitable (designed for unstructured narrative data, underperforms standard RAG on math textbooks, brittle to updates). Full Agentic RAG not suitable (compound error risk, over-complex for direct curriculum queries). Recommendation: keep hybrid wiki+RAG architecture, upgrade RAG layer with hierarchical indexing, hybrid search, query rewriting, and contextual retrieval.

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
