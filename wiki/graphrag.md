# GraphRAG

**Summary**: Microsoft Research's RAG variant that builds an LLM-generated knowledge graph from a corpus, then uses graph structure and community summaries to answer holistic or multi-hop questions that defeat standard vector search.

**Sources**: `raw/GraphRAG_ A new approach for discovery using complex information.md`, `raw/GraphRAG_Microsoft introduction.md`

**Last updated**: 2026-05-09

---

## What It Is

GraphRAG replaces the vector search step in standard RAG with a structured knowledge graph. Instead of finding similar text chunks, it traverses entity-relationship structure to find connected information across documents — even when no single chunk mentions all the relevant pieces (source: Microsoft Research Blog).

## How It Works

### Indexing Phase
1. The LLM reads the entire corpus and extracts all **entities** (people, places, organizations, concepts) and **relationships** between them.
2. Entities and relationships form a **knowledge graph** — each node is an entity, each edge is a relationship.
3. The graph is clustered hierarchically using the **Leiden algorithm**, grouping entities into semantic communities.
4. The LLM generates a **summary for each community** at multiple levels of abstraction (bottom-up).

### Query Phase
Four query modes are available (source: Microsoft GraphRAG docs):
- **Global Search**: Answers holistic questions ("What are the top themes in this dataset?") using community summaries. Works across the entire corpus.
- **Local Search**: Answers entity-specific questions by fanning out to neighbors in the graph.
- **DRIFT Search**: Like Local Search, but also uses community context.
- **Basic Search**: Falls back to standard top-k vector search when that's sufficient.

## Where GraphRAG Excels

GraphRAG was built for **unstructured, messy, multi-perspective text** — particularly news articles, social media, and narrative data. The original demo used the VIINA dataset (Ukrainian/Russian war news, June 2023).

**Connecting the dots**: When answering "What has Novorossiya done?", standard RAG retrieved zero relevant chunks. GraphRAG traversed entity-relationship links from "Novorossiya" to find connected activities across many documents (source: Microsoft Research Blog).

**Whole-dataset reasoning**: When asked "What are the top 5 themes?", standard RAG returned irrelevant results (keying on the word "theme"). GraphRAG used community summaries to surface the actual dataset-wide themes: conflict, political entities, infrastructure, etc. (source: Microsoft Research Blog).

## Where GraphRAG Fails

### Structured Educational Content
A 2025 paper ("Comparing RAG and GraphRAG for Page-Level Retrieval Question Answering on Math Textbook", arXiv:2509.16780) compared GraphRAG vs. standard RAG on an undergraduate mathematics textbook using 477 QA pairs. Standard RAG achieved **higher retrieval accuracy (Top 1/3/5/10) and higher F1 scores** across five top embedding models. GraphRAG retrieved excessive, less precise content because entity-based graph traversal finds more than needed when the document is already well-structured. (source: arXiv:2509.16780)

Textbooks are **not** the kind of messy, multi-source data GraphRAG is designed for. The answers are in clearly bounded sections — vector search finds them well without graph traversal.

### Brittleness to Updates
GraphRAG requires **re-indexing significant graph portions** when source material changes. For a living curriculum (new textbooks, updated exam papers), this is a serious maintenance burden. Standard RAG only requires re-embedding changed chunks.

### Cost and Complexity
Building the knowledge graph requires many LLM calls over the entire corpus during indexing. This is expensive both in API costs and time. Every curriculum update restarts this process.

### Auto-Extraction Noise
LLM-based entity extraction produces errors. On clean, technical educational content (formulas, theorems, step-by-step proofs), the auto-extracted graph will contain noise and misattributions. A manually maintained wiki-link structure is more reliable.

## GraphRAG vs. Our Manual Wiki Links

Our [[knowledge-base-architecture|curated wiki]] already achieves what GraphRAG builds automatically — but with higher quality:

| Aspect | GraphRAG (auto) | Our wiki (manual) |
|---|---|---|
| Entity extraction | LLM-generated, noisy | Human-curated, accurate |
| Relationships | Auto-detected, approximate | Explicit wiki-links, intentional |
| Community summaries | LLM-generated | Human-written concept pages |
| Maintenance on update | Re-index large graph portions | Update relevant pages only |
| Cost | High (many LLM calls at index time) | Low (incremental human effort) |

The conclusion: **our manual wiki IS a knowledge graph**, maintained with human judgment rather than LLM auto-extraction. GraphRAG would replicate this at lower quality and higher cost.

## When You Would Use GraphRAG

GraphRAG is the right tool when:
- The corpus is large, unstructured, and multi-source (news, social media, enterprise communications)
- Questions require synthesizing across many documents to find hidden connections
- The corpus is relatively stable (infrequent updates)
- Global/thematic discovery questions are a primary use case
- You don't have domain experts available to curate a structured knowledge base manually

For an academic knowledge base with structured curriculum content, curated by domain experts, updated periodically — GraphRAG is not the right tool.

## Related Pages

- [[rag]]
- [[agentic-rag]]
- [[knowledge-base-architecture]]
- [[rag-architecture-comparison]]
