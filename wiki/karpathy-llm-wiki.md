# Karpathy LLM Wiki

**Summary**: Andrej Karpathy's pattern for building a personal knowledge base where an LLM acts as a "compiler" — reading raw source documents and producing a structured, interlinked wiki that compounds knowledge over time.

**Sources**: Web research (April 2026). Original gist: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

**Last updated**: 2026-05-08

---

## The Core Idea

Most LLM + document systems use [[rag|RAG]]: retrieve raw chunks at query time, generate an answer, and forget everything. Nothing accumulates.

Karpathy's insight: treat the LLM as a **compiler**. Instead of searching raw documents on every query, the LLM reads them once and produces a structured, interlinked wiki. Future queries read the wiki — not the raw sources.

Knowledge compounds because good answers can be filed back into the wiki as new pages.

## Architecture

Three layers:

1. **Raw sources** — immutable input documents (PDFs, articles, notes). Never modified.
2. **Wiki** — LLM-generated summary pages, concept pages, and cross-links in plain Markdown.
3. **Schema** — a `CLAUDE.md` (or equivalent) that turns the LLM agent into a disciplined wiki maintainer with explicit rules for ingestion, formatting, and linting.

## Compilation Step

The LLM reads raw sources and writes:
- A summary page per source
- Concept pages for each major idea
- `[[wiki-links]]` connecting related pages
- An `index.md` table of contents
- An append-only `log.md`

A single source may touch 10–15 pages. That is normal.

## Compounding Mechanism

When the user asks a question, the LLM reads the wiki (not raw sources) and synthesizes an answer. If the answer is valuable and non-obvious, it gets filed back as a new wiki page. Over time the wiki becomes richer without growing the raw source collection.

## Linting / Health Checks

Periodic audit passes where the LLM scans for:
- Contradictions between pages
- Orphan pages (no inbound links)
- Concepts mentioned but lacking their own page
- Claims that may be outdated
- Pages that don't follow the format

This is the primary defense against hallucinations baking into the wiki as facts.

## Comparison to RAG

| Aspect | RAG | LLM Wiki |
|---|---|---|
| Knowledge accumulation | None — stateless | Yes — compounds over time |
| Query target | Raw document chunks | Pre-synthesized wiki pages |
| Auditability | Hard — chunks are fragments | Easy — plain Markdown files |
| Hallucination risk | At query time | At compilation time (mitigated by lint) |
| Setup complexity | High (vector DB, embeddings) | Low (plain files + agent instructions) |

See [[rag]] for a full treatment of RAG.

## Key Risk

Hallucinations can get **baked into wiki pages as facts** and propagate across linked pages via wiki-links. The lint step is the main mitigation — periodic audits spot-check generated pages against raw sources.

## Historical Inspiration

Karpathy connects the idea to **Vannevar Bush's Memex (1945)** — a vision of a private, curated personal knowledge store with "associative trails" between documents. The public web drifted far from that vision. The LLM Wiki is arguably closer to Bush's original idea than anything built in the 80 years since.

## Related pages

- [[rag]]
- [[vector-database]]
