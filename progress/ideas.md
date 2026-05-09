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
