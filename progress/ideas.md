# Ideas & Decisions Log

Running record of ideas explored and decisions made. Append-only — never delete entries.

**Last updated**: 2026-05-15

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

---

## 2026-05-12 (session 3)

### Architecture further validated by practical RAG literature
Ingested "A Practical Approach to Retrieval Augmented Generation Systems" (Allahyari & Yang). Key validation: the "Document Summary Index" (Dense Hierarchical Retrieval) technique described in the literature is structurally identical to our hybrid two-layer architecture. We arrived at a recognized best practice independently.

**Status**: Noted — no decision change required.

---

### New finding: "Lost in the Middle" production risk
Research finding (Liu et al., 2023): LLMs consistently ignore context in the middle of long retrieved-context windows. Relevant chunks retrieved at positions 3–7 (out of 10) may be effectively invisible to the model. Fix: reorder documents before synthesis. This is a low-cost implementation step that should be built into the RAG layer from the start.

**Status**: Added to `wiki/knowledge-base-architecture.md` as an architectural note. Not added to roadmap (already captured under "RAG layer upgrades" in Phase 3).

---

### Idea: Chunk size evaluation — deferred
The practical literature shows chunk size is non-trivial and must be evaluated empirically using faithfulness + relevancy metrics. For Thai math/science content, standard defaults may not apply. However, owner confirmed this is outside current time constraints.

**Status**: Deferred. Noted in `wiki/rag-pipeline-implementation.md` as a known concern. Not added to roadmap.

---

## 2026-05-14 (session 4)

### Decision: Concept page format — free-form content body
Rejected a rigid section structure (Definition → Key formulas → Explanation) in favor of a fixed metadata shell with a free-form content body. Different concepts and subjects need different explanatory approaches. Pages can also include pedagogical framing, common misconceptions, and teaching notes — content that goes beyond academic facts and differentiates the AI from generic online sources.

**Status**: Decided. See `wiki/concept-page-format.md`.

---

### Decision: IPST as conflict-resolution authority only
IPST chapter structure does not dictate page organization or curriculum anchor. IPST's role is solely: when sources present conflicting academic facts, IPST takes precedence. Company curriculum structure is the organizational anchor. Conflicts noted inline in content body where they arise.

**Status**: Decided. See `wiki/concept-page-format.md`.

---

### Decision: Parameterized URL references for simulation linking
Even for the MVP (simple URL references), simulation links use a parameterized scheme (e.g., `?state=amplitude`) rather than bare links. This is forward-compatible with the long-term requirement: simulations may need to start in different states depending on the problem context the student is working on (e.g., simple harmonic motion starting at amplitude vs. equilibrium).

Long-term stateful simulation strategy is still open — the URL parameter approach is the MVP placeholder.

**Status**: MVP approach decided. Long-term approach deferred.

---

## 2026-05-15 (session 5)

### Decision: Simulation embedding — two-phase strategy
Phase 1 (MVP/testing): `<iframe>` embed — instantly playable in chat without leaving the page. Testing platform supports raw HTML in markdown.
Phase 2+ (production): custom `simulation` JSON code block, interpreted by a renderer on the chat platform. The renderer fetches the simulation from a dedicated sim repo and renders it inline. This approach is markdown-safe (production strips iframes for security), decoupled from specific URLs, and forward-compatible with parameterized initial state.
JSON schema and sim repo structure deferred until the renderer is built.

**Status**: Phase 1 decided. Phase 2 approach defined, schema deferred. See `wiki/concept-page-format.md`.

---

### Decision: `kb/` directory established as the actual academic KB layer
Created `kb/` as the root of the academic concept pages — distinct from `wiki/` (which is the research wiki about this project). Structure: subject-level folders (`physics/`, `math/`, `biology/`, `chemistry/`) with a master `index.md` and `log.md`. One level of nesting chosen over deep taxonomy — the curriculum anchor field in page metadata encodes the chapter/unit hierarchy.

Currently co-located with the research wiki in this repo for MVP convenience. Flagged for migration to a separate repo once the content layer matures.

**Status**: Decided. `kb/` directory created.

---

### Idea: User history caching for school deployment
Students repeatedly ask the same curriculum questions. A vector-database-backed Q&A cache could bypass embedding + retrieval + LLM generation for repeated queries — significant cost and latency reduction at school scale.

**Status**: Noted as Phase 4+ optimization. Added to `wiki/advanced-rag-techniques.md` and `wiki/knowledge-base-architecture.md`.
