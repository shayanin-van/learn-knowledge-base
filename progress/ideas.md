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

### Open question: LLM Wiki vs RAG vs hybrid for the actual project
The wiki uses the LLM Wiki pattern for *this* research. But the *project being researched* (an academic knowledge base) may need a different architecture depending on scale, audience, and update frequency.

**Status**: Open — needs project scope discussion.
