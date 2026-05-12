# Advanced RAG Techniques

**Summary**: Production-grade improvements to the standard RAG pipeline — chunk decoupling, the "Lost in the Middle" problem, hybrid retrieval, query rewriting, query routing, and user history caching.

**Sources**: `raw/A Practical Approach to Retrieval Augmented Generation Systems/` (Allahyari & Yang), `raw/A modular Agentic RAG built with LangGraph...md`, `raw/RAG and Agent - Snippet from the book - AI Engineering by Huyen.pdf`

**Last updated**: 2026-05-12

---

## Overview

Standard RAG (query → retrieve → generate) has well-known failure modes in production: it retrieves the wrong chunks, loses context in long responses, and doesn't adapt to how users phrase questions. These techniques address specific failure modes. They can be adopted independently — none requires the full agentic orchestration approach (see [[agentic-rag]] for why that's avoided).

---

## 1. Chunk Decoupling: Retrieval Chunks vs. Synthesis Chunks

The chunk that is best for **finding** information is not always the best chunk for **answering** from it.

- Small chunks embed well and retrieve precisely — they carry a focused semantic signal
- But small chunks often lack enough surrounding context for the LLM to generate a complete, coherent answer
- Large chunks contain more context but embed poorly — the semantic signal is diluted, so retrieval quality drops

### Document Summary Index (Dense Hierarchical Retrieval)

The solution is a two-level data structure:

1. **Summary level** — one embedding per document (or section), representing the document's overall topic
2. **Chunk level** — many fine-grained chunk embeddings per document, linked back to their parent document

At query time:
1. Retrieve the most relevant **document** using summary-level embeddings
2. Within that document, retrieve the most relevant **chunks** for context

This approach is directly aligned with the hybrid wiki + RAG architecture of this project: curated concept pages act as the summary level; chunks of those pages serve as the retrieval level.

Research reference: Dense Hierarchical Retrieval (Y. Liu et al., 2021; Zhao et al., 2022) — source: Allahyari & Yang.

### Sentence Window Expansion

An alternative: index at sentence granularity for maximum precision, but when a sentence is retrieved, include the surrounding sentences (a configurable "window") before passing to the LLM. This gives fine-grained retrieval with richer generation context.

LlamaIndex implements this via `SentenceWindowNodeParser` and `MetadataReplacementNodePostProcessor`.

---

## 2. The "Lost in the Middle" Problem

When many chunks are retrieved and passed to the LLM as a long context, **the LLM tends to ignore information in the middle of that context** (Liu et al., 2023 — source: Allahyari & Yang).

Key finding: performance peaks when relevant information is at the **beginning or end** of the context window. Accuracy drops significantly when relevant chunks are buried in the middle. This holds even for LLMs with longer context windows — increasing context size does not solve the problem.

### Implications for our project

If a student asks a question and top-10 chunks are retrieved, the 3rd–7th most relevant chunks may effectively be ignored by the LLM. This means the system's answer quality degrades as more chunks are retrieved, contrary to intuition.

### Fix: Document Reordering

Reorder retrieved chunks before passing to the LLM: place the most relevant chunks at the **top and bottom**, the least relevant in the **middle**.

LangChain provides `LongContextReorder` for this. Haystack provides `LostInTheMiddleRanker` which implements the same strategy.

This is a low-cost, high-impact fix that should be included in the RAG layer implementation.

---

## 3. Hybrid Retrieval

See [[agentic-rag]] for the full description of hybrid search (BM25 + semantic, merged with RRF).

Additional context from the practical guide: keyword-based search specifically outperforms semantic search in these cases:
- **Highly specific queries** — e.g., a student searching for "สมการกำลังสอง" (quadratic equation) — exact term match is critical
- **Niche domain jargon** — Thai academic terminology and subject-specific abbreviations won't always have good semantic embeddings
- **Short documents** — our concept pages are relatively short; semantic models work better on longer text

**Merging strategies** when combining keyword + semantic results:

| Strategy | When to use |
|---|---|
| Concatenation | All results matter; ranking is not critical |
| Reciprocal Rank Fusion (RRF) | Results need to be ranked; documents appearing in both lists are elevated |
| Score-based merging | One retriever is more trusted than the other |

RRF is the recommended default for our use case — it doesn't require comparable score scales across the two retrieval methods.

Frameworks with native hybrid retrieval support: ElasticSearch, Weaviate, Haystack.

---

## 4. Query Rewriting

See [[agentic-rag]] for the core description.

Additional techniques from the practical guide:

**Synonym and conceptual expansion** — the LLM rewrites "renewable energy sources" to also include "green energy sources", "sustainable energy sources". For our project: a student asking about "แรงลัพธ์" (resultant force) might benefit from expansion to include "กฎนิวตัน" (Newton's law) related terms.

**Multilingual query expansion** — an LLM can translate and expand queries across languages. This is directly relevant when Thai students mix Thai/English in their queries (e.g., "อธิบาย momentum ให้หน่อย").

**Multi-step sub-query generation** — complex questions can be broken into sequential sub-queries:
- "ความแตกต่างระหว่าง kinetic energy กับ potential energy" → sub-query 1: "kinetic energy คืออะไร" → sub-query 2: "potential energy คืออะไร" → merge

---

## 5. Query Routing

Query routing selects the retrieval mechanism dynamically based on the type of query.

Rather than sending every query through the same pipeline, a router classifies the query's intent and dispatches it to the appropriate tool:

| Query type | Route to |
|---|---|
| "What is X?" | Definition/concept lookup |
| "How do I solve X?" | Worked example retrieval |
| "Summarize topic X" | Summarization pipeline |
| "Which exam questions cover X?" | Past exam index |

### Implementation approaches

1. **Zero-shot classifier** — a lightweight model classifies the query into predefined intent categories
2. **Prompt-based routing** — the LLM itself classifies the query using a prompt template
3. **Rule-based routing** — regex or keyword matching for simple cases (e.g., queries starting with "สรุป" are routed to summarization)

LlamaIndex provides `RouterQueryEngine` which accepts multiple `QueryEngineTool` objects, each backed by a different index.

For our project, routing between a definition/concept index and an exam-problem index is a natural application — though this is a Phase 3+ implementation concern.

---

## 6. User History and Response Caching

Repeated queries can bypass the full retrieval pipeline using a cached Q&A store.

### Problem
In a school context, many students ask the same questions: "Newton's first law", "mitosis vs. meiosis", "แก้โจทย์สมการ" (solving equations). Re-running embedding + retrieval + LLM generation for each identical query is wasteful.

### Simple approach: key-value cache
Store previous question → answer pairs. On each new query, check if an identical (or near-identical) question exists. If yes, return the cached answer directly.

### Scalable approach: vector-database cache
Store previous question **embeddings** in a vector database. On each new query, compute the embedding and check for a high-similarity match (above a threshold). If found, return the cached response. This handles paraphrased questions, not just exact duplicates.

**Benefits**:
- Reduces per-query LLM API costs
- Significantly reduces latency for common questions
- Improves response consistency (same question always gets the same answer)

**Risk**: Cached answers become stale if the knowledge base is updated. Cache entries should be invalidated when source pages are modified.

For our project, student query patterns will be highly repetitive across curriculum topics. A caching layer is a practical cost-reduction measure for Phase 4+.

---

## What's Already Covered in Related Pages

The following techniques are described in [[agentic-rag]] and are not duplicated here:
- Hierarchical indexing (parent/child chunks)
- Contextual retrieval (chunk-with-context augmentation)
- Hybrid search (BM25 + dense with RRF)
- Query rewriting (core description)
- Context compression

---

## Related pages

- [[rag]]
- [[rag-pipeline-implementation]]
- [[agentic-rag]]
- [[knowledge-base-architecture]]
