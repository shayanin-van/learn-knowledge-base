# RAG Pipeline Implementation

**Summary**: Practical details of building a RAG pipeline — how documents are ingested, chunked, embedded, stored, and used for generation. Covers key trade-offs that affect system quality in production.

**Sources**: `raw/A Practical Approach to Retrieval Augmented Generation Systems/` (Allahyari & Yang)

**Last updated**: 2026-05-12

---

## The Two-Stage Pipeline

A RAG system has two separate pipelines that run at different times:

**Data ingestion pipeline** (run once, or when knowledge changes):
1. Extract text from source documents
2. Split text into chunks
3. Embed each chunk into a vector
4. Store vectors + metadata in a vector database

**Query pipeline** (run on every user question):
1. Embed the user's query using the same model
2. Retrieve top-k similar chunks from the vector database
3. Build a prompt: query + retrieved chunks as context
4. Pass the prompt to the LLM
5. Return the generated response

---

## Text Extraction

For PDF documents, extraction tools include PyPDF2, pdfminer.six, and `unstructured`. Scanned PDFs (image-based) require OCR (e.g., pytesseract) first.

For our project, documents are curated Markdown wiki pages — extraction is trivial. The ingestion challenge is elsewhere (chunking, embedding).

---

## Text Splitting

How you split documents into chunks is one of the highest-impact decisions in a RAG system.

### Character-based splitting
Splits by a fixed number of characters. Preserves fine-grained detail but produces variable-quality chunks — a split can fall mid-sentence, mid-formula, or mid-derivation, losing context.

### Token-based splitting
Splits by token count, respecting the LLM's context window constraints. Better than character splitting for most cases. Most frameworks (LangChain, LlamaIndex) default to this with a configurable `chunk_size` and `chunk_overlap`.

### Chunk overlap
Adding overlap between adjacent chunks (e.g., 10–20% of chunk size) ensures that sentences near chunk boundaries appear in both chunks, reducing the chance that a key sentence is cut off from its context.

### Practical guidance
- Chunk size = 1024 tokens is a commonly tested starting point; experiments in the source material showed it peaking on faithfulness + relevancy metrics
- There is **no universal optimal chunk size** — it depends on document type, query type, and the LLM being used
- For math/science content with formulas and derivations, standard token splitting may break critical sequences; semantic splitting (by section headers, paragraphs) may be better

See [[advanced-rag-techniques]] for the "retrieval chunk vs synthesis chunk" approach, which decouples chunk size from context size.

---

## Embedding Models

Embedding converts text into a numerical vector that captures semantic meaning. The choice of embedding model affects retrieval quality significantly.

### Key trade-offs

| Factor | Cloud API (OpenAI, Cohere) | Self-hosted (HuggingFace) |
|---|---|---|
| Cost | Pay-per-token; expensive at scale | Free after compute cost |
| Latency | Higher (network round trip) | Lower (local) |
| Quality | High; regularly updated | Varies; requires evaluation |
| Maintenance | None | Requires model management |

### Multilingual
For Thai text, embedding models must handle Thai characters and Thai language semantics. Options include:
- **Multilingual BERT (mBERT)** — explicitly designed for multilingual datasets
- **multilingual-e5** — strong cross-lingual performance
- **OpenAI text-embedding-3-large** — supports multilingual, high quality

Evaluating which model performs best on Thai academic content requires domain-specific benchmarking. The **MTEB leaderboard** (Hugging Face) benchmarks models across 8 retrieval and embedding tasks and is the standard reference for comparing options.

This is an open question for our project — see [[knowledge-base-architecture]].

---

## Vector Databases

Vector databases store embeddings and support fast approximate nearest neighbor (ANN) search. Common options:

| Database | Hosting | Notes |
|---|---|---|
| Qdrant | Self-hosted or cloud | Open source; fast; free cloud tier |
| Weaviate | Self-hosted or cloud | Open source; supports hybrid search natively |
| Pinecone | Cloud only | Fully managed; no ops overhead |
| pgvector | Self-hosted (Postgres) | Embeds into existing Postgres; good for small-medium scale |
| Milvus | Self-hosted | Enterprise-grade; complex to operate |
| Chroma | Self-hosted | Lightweight; good for development/POC |

For production use, the key requirements are scalability, latency, and cost-efficiency. Qdrant and Weaviate are strong open-source choices. Weaviate has native hybrid search (BM25 + vector) which aligns with the hybrid retrieval approach planned for our project.

---

## Generation Component

Once top-k chunks are retrieved, they are assembled into a prompt alongside the user's query. The LLM then generates a response grounded in that context.

LangChain and LlamaIndex both provide pre-built chains for this:
- `RetrievalQA` — retriever + prompt template + LLM in one pipeline
- `RetrievalQAWithSourcesChain` — same, but returns source chunk metadata alongside the answer (useful for citations)

For our project, returning source metadata is important — students should see which concept page their answer came from.

---

## Production Challenges

Simple RAG prototypes are straightforward to build. Production deployments face:

- **Low precision**: top-k retrieval includes irrelevant chunks, causing hallucination or confused answers
- **Low recall**: the right chunk exists but isn't retrieved because the query phrasing doesn't match the chunk's embedding
- **Obsolete information**: retrieved chunks may reflect outdated knowledge if the index isn't kept current
- **Scalability**: as the corpus grows, index maintenance and retrieval latency require active management
- **Evaluation**: measuring whether the system is actually performing well requires defined metrics

See [[advanced-rag-techniques]] for techniques that address precision, recall, and context quality.

---

## Related pages

- [[rag]]
- [[vector-database]]
- [[advanced-rag-techniques]]
- [[knowledge-base-architecture]]
