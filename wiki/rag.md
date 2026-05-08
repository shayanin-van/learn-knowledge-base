# RAG (Retrieval-Augmented Generation)

**Summary**: An AI architecture that enhances LLM responses by retrieving relevant documents from an external knowledge base at query time, grounding answers in real data without retraining the model.

**Sources**: Web research (2025–2026). Key sources: Google Cloud, IBM, AWS, NVIDIA, RAGFlow blog.

**Last updated**: 2026-05-08

---

## What Is RAG?

RAG combines two capabilities:
- **Retrieval** — finding relevant documents from an external knowledge base
- **Generation** — an LLM producing a response augmented by those retrieved documents

The LLM does not need to be retrained as knowledge changes. The knowledge base is updated independently, and the LLM queries it at runtime.

## Core Pipeline

1. **Ingestion** — documents are chunked into smaller pieces, converted into text embeddings, and stored in a [[vector-database]]
2. **Retrieval** — at query time, the user's question is embedded and semantically similar chunks are fetched from the vector database
3. **Augmented generation** — the retrieved chunks are injected into the LLM's context alongside the query; the LLM generates a grounded response

## Why RAG Exists

LLMs have a fixed training cutoff and no access to private or proprietary data. RAG solves both problems:
- Keeps responses current without retraining
- Lets organizations use internal documents (wikis, manuals, databases) without exposing them to model training

## Benefits

- **Reduces hallucinations** — the LLM answers from retrieved facts rather than pattern-matching from training
- **Data privacy** — the knowledge base stays external; access can be revoked at any time
- **Cost-effective** — no fine-tuning or retraining required
- **Auditable** — retrieved chunks can be shown as citations

## Limitations

- **Stateless** — each query retrieves from scratch; nothing is learned or accumulated between queries
- **Retrieval noise** — irrelevant chunks can be retrieved and confuse the LLM
- **Chunking problem** — splitting documents into chunks loses context; a question requiring synthesis across five chunks is harder to answer correctly
- **Infrastructure overhead** — requires maintaining a vector database and embedding pipeline

The stateless limitation is the central critique in [[karpathy-llm-wiki]], which proposes a compiled wiki as an alternative.

## RAG vs. LLM Wiki

| Aspect | RAG | LLM Wiki |
|---|---|---|
| Knowledge accumulation | None | Compounds over time |
| Query target | Raw chunks | Synthesized wiki pages |
| Infrastructure | Vector DB + embeddings | Plain Markdown files |
| Best for | Large, dynamic document corpora | Curated, evolving personal knowledge |

These are not mutually exclusive. For large-scale, dynamic corpora RAG is often the right tool. For personal or departmental knowledge that grows through exploration, the LLM Wiki pattern may serve better.

## RAG in 2025

As of 2025, RAG has solidified as a cornerstone of enterprise AI. Trends include:
- **Agentic RAG** — LLMs dynamically orchestrate multiple retrieval steps
- **Multi-modal retrieval** — embedding images, audio, and video alongside text
- **Graph RAG** — combining vector similarity with knowledge graph traversal for richer context

## Related pages

- [[vector-database]]
- [[karpathy-llm-wiki]]
