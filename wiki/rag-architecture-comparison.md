# RAG Architecture Comparison

**Summary**: Analysis of three RAG architecture approaches — Standard RAG, GraphRAG, and Agentic RAG — evaluated against the specific constraints of a Thai academic knowledge base for student exam prep.

**Sources**: `raw/GraphRAG_ A new approach for discovery using complex information.md`, `raw/GraphRAG_Microsoft introduction.md`, `raw/A modular Agentic RAG built with LangGraph — learn Retrieval-Augmented Generation Agents in minutes..md`, `raw/RAG and Agent - Snippet from the book - AI Engineering by Huyen.pdf`

**Last updated**: 2026-05-09

---

## The Decision Context

We are evaluating architectures for a retrieval layer that powers an AI companion serving Thai high school students preparing for O-NET/PAT/TCAS. The core constraints:

1. **High stakes**: Wrong answers on exam prep have real consequences. Reliability and correctness beat flexibility.
2. **Structured content**: Source material is curriculum-defined textbooks (IPST), past exam papers, and curated concept explanations — not messy multi-source narrative data.
3. **Defined audience**: Students asking direct curriculum questions ("define acceleration", "solve this kinematics problem"), not analysts discovering hidden patterns.
4. **Multilingual**: Thai and English content must both be handled.
5. **Living curriculum**: Content updates when IPST releases new materials or exam papers change.
6. **Existing curated layer**: We already have a wiki of hand-crafted concept pages with wiki-links — this is our quality/trust layer.

---

## Architecture A: Standard RAG (Baseline)

Retrieve top-k similar chunks by vector embedding, pass to LLM for generation.

### Fit Assessment

| Criterion | Rating | Notes |
|---|---|---|
| Reliability | Medium | Single-pass retrieval, predictable failure modes |
| Structured content | Good | Works well on well-bounded textbook sections |
| Student query types | Good | Direct questions answered with direct chunk retrieval |
| Multilingual | Medium | Depends on embedding model choice |
| Update cost | Low | Re-embed changed chunks only |
| Complexity | Low | Easy to debug, audit, and improve |

### Gaps
- Colloquial student queries may not retrieve the right chunks
- No awareness of concept relationships (e.g., "velocity" → "acceleration" → "Newton's laws")
- Chunking strategy can cause relevant content to be split across chunk boundaries

---

## Architecture B: GraphRAG

Build an LLM-extracted knowledge graph from the corpus; use graph traversal and community summaries for retrieval.

### Fit Assessment

| Criterion | Rating | Notes |
|---|---|---|
| Reliability | Low-Medium | Auto-extraction introduces noise; graph traversal can retrieve excessive content |
| Structured content | **Poor** | Evidence shows GraphRAG underperforms standard RAG on math textbooks (lower F1, lower precision) |
| Student query types | Poor | Global/thematic discovery queries are rare in exam prep; local entity lookup is what standard RAG already does |
| Multilingual | Poor | LLM entity extraction on Thai technical text is unproven and likely error-prone |
| Update cost | **High** | Curriculum change → re-index significant graph portions → expensive |
| Complexity | High | Indexing pipeline requires many LLM calls; debugging failures is hard |

### Key Evidence Against
- arXiv:2509.16780 (2025), 477 QA pairs from an undergraduate math textbook: standard RAG outperforms GraphRAG on retrieval accuracy (Top 1/3/5/10) and F1 score across five top embedding models. GraphRAG retrieves too much due to entity-based traversal, reducing answer quality.
- Our curated wiki already performs what GraphRAG builds automatically — with human accuracy instead of LLM extraction noise.
- GraphRAG is designed for "narrative private data" (their term): news articles, social media, enterprise communications. Not textbooks.

### Verdict: Not recommended.

---

## Architecture C: Full Agentic RAG

LLM agent dynamically plans multi-step retrieval, self-corrects, uses multiple tools.

### Fit Assessment

| Criterion | Rating | Notes |
|---|---|---|
| Reliability | **Low** | Compound error risk: 95% accuracy/step → 60% over 10 steps (Huyen). High-stakes context amplifies this. |
| Structured content | Medium | Agent can handle it, but so can simpler approaches |
| Student query types | Mixed | Most queries are direct and don't need multi-hop; complex multi-topic queries could benefit |
| Multilingual | Medium | Depends on underlying models |
| Update cost | Low | Underlying index updates are same as standard RAG |
| Complexity | **High** | Debugging agent loops, testing behavior, auditing failures — all harder than a fixed pipeline |

### Key Evidence Against
- Most student queries are direct: "what is Newton's second law?" doesn't need an orchestrator loop.
- Compound error in high-stakes contexts is dangerous. A single-pass system that fails clearly is preferable to a multi-step system that fails silently.
- Agent complexity adds latency inappropriate for a homework-help context.

### Verdict: Not recommended as the primary architecture. Specific component techniques are valuable.

---

## Architecture D: Hybrid Curated Wiki + Upgraded RAG (Current Decision)

Hand-curated wiki pages as the quality/trust layer. RAG runs on top of those curated pages. Upgrade the RAG layer with best-practice techniques from Agentic RAG research.

### Fit Assessment

| Criterion | Rating | Notes |
|---|---|---|
| Reliability | **High** | Human-curated content guarantees quality; predictable retrieval pipeline |
| Structured content | **High** | Wiki pages are structured exactly as needed for curriculum content |
| Student query types | **High** | Direct questions answered well; curated explanations are pedagogically sound |
| Multilingual | Good | Thai-language pages can be authored directly; embedding model choice separate |
| Update cost | Low-Medium | Update relevant wiki pages only; re-embed changed pages |
| Complexity | Low-Medium | Fixed retrieval pipeline with optional enhancements |

### Upgraded RAG Layer (borrowing from Agentic RAG)

These techniques improve the RAG layer without full agent complexity:

| Technique | How it helps us |
|---|---|
| **Hierarchical indexing** | Index wiki sections as child chunks, full pages as parent chunks — precision search + full context on retrieval |
| **Query rewriting** | Rewrite colloquial Thai student questions into embedding-search-friendly forms before retrieval |
| **Hybrid search** | Dense (semantic) + sparse (BM25) combined with RRF — catches formula names AND concept meaning |
| **Contextual retrieval** | Add 50–100 token AI context to each chunk at index time (Anthropic technique) — improves short chunk quality |

### Our Wiki IS a Knowledge Graph

The key insight: our manually curated wiki with wiki-links is a knowledge graph — it just uses human judgment instead of LLM auto-extraction.

| GraphRAG component | Our equivalent |
|---|---|
| Entity extraction | Human-authored concept pages |
| Relationship edges | Wiki-links ([[page-name]]) |
| Community summaries | Category pages and the index |
| Provenance | Source citations in every page |

We get the benefits of structured knowledge without the extraction noise, update brittleness, or indexing cost.

### Verdict: **Recommended.**

---

## Summary Scorecard

| Criterion | Standard RAG | GraphRAG | Agentic RAG | Hybrid Wiki + Upgraded RAG |
|---|---|---|---|---|
| Reliability for high-stakes | Medium | Low | Low | **High** |
| Fit for structured curriculum | Good | **Poor** | Medium | **High** |
| Fit for student query types | Good | Poor | Mixed | **High** |
| Maintenance cost | Low | **High** | Low | Low-Medium |
| System complexity | Low | High | **High** | Low-Medium |
| Evidence from similar domains | — | Math textbook: worse than baseline | — | — |
| **Overall recommendation** | Baseline | ❌ Reject | ❌ Reject as whole | ✅ **Adopt** |

---

## Recommendation

**Keep the hybrid curated wiki + RAG architecture. Upgrade the RAG layer with four specific techniques: hierarchical indexing, query rewriting, hybrid search, and contextual retrieval.**

This decision is now better-supported than before: we have specific evidence (GraphRAG underperforms on math textbooks, agentic compound error risk) and have identified the exact techniques worth borrowing. The architecture is not "standard RAG" — it is a purposefully upgraded RAG layer running on a human-quality knowledge graph (the wiki).

The remaining open question is the RAG layer implementation: which Thai-capable embedding model, what chunking sizes for math content, how to represent LaTeX formulas in embeddings.

## Related Pages

- [[rag]]
- [[graphrag]]
- [[agentic-rag]]
- [[vector-database]]
- [[knowledge-base-architecture]]
- [[karpathy-llm-wiki]]
