# Vector Database

**Summary**: A specialized database that stores data as high-dimensional embedding vectors, enabling fast semantic similarity search — the retrieval engine that powers most [[rag]] systems.

**Sources**: Web research (2025–2026). Key sources: DEV Community, IJCT journal, IBM, NVIDIA.

**Last updated**: 2026-05-08

---

## What Is a Vector Database?

A vector database stores data as **embedding vectors** — numerical representations of meaning in high-dimensional space (often hundreds to thousands of dimensions). Two semantically similar pieces of text will have vectors that are close together in this space.

This enables **semantic similarity search**: find documents that are *meaningfully* similar to a query, not just ones that share the same keywords.

## How It Works

1. **Embedding** — a text (or image, audio, etc.) is passed through an embedding model and converted to a fixed-length vector
2. **Indexing** — the vector is stored in the database alongside metadata and the original content
3. **Query** — at search time, the query is embedded the same way, and the database returns the stored vectors nearest to it (by cosine similarity or Euclidean distance)
4. **Retrieval** — the top-k most similar documents are returned for use in the LLM context

## Comparison to Traditional Databases

| Feature | Traditional DB | Vector DB |
|---|---|---|
| Search type | Exact / keyword match | Semantic / similarity |
| Data format | Structured rows | Unstructured embeddings |
| Query language | SQL | Nearest-neighbor search |
| Use case | Transactions, records | Meaning-based retrieval |

## Common Vector Databases (2025)

| Database | Best for |
|---|---|
| **ChromaDB** | Fast local prototyping, easy setup |
| **FAISS** (Meta) | Lightweight, in-process, no server needed |
| **Milvus** | Large-scale production deployments |
| **Pinecone** | Managed cloud service, minimal ops overhead |
| **pgvector** | Adding vector search to existing PostgreSQL |
| **Weaviate** | Graph + vector hybrid search |
| **Qdrant** | Rust-based, high performance |

For small-scale deployments, the choice of vector database is rarely a performance bottleneck — ease of deployment and infrastructure fit matter more than raw speed.

## Role in RAG

Vector databases are the retrieval engine inside a [[rag]] system. The pipeline is:

```
documents → embedding model → vector DB (index)
                                      ↑
query → embedding model → similarity search → top-k chunks → LLM context
```

Without a vector database, RAG would fall back to keyword search, losing semantic understanding.

## Multi-modal Support

Modern vector databases support embeddings beyond text:
- Images
- Audio
- Video
- Code

This enables multi-modal RAG systems that can retrieve the right media alongside text.

## Limitations

- **Embedding quality** — retrieval is only as good as the embedding model; poor embeddings mean poor recall
- **Chunk context loss** — documents are split into chunks before embedding, which can sever important context
- **Infrastructure cost** — running and maintaining a vector database adds operational complexity
- **No accumulation** — like [[rag]] itself, the vector database is a retrieval system, not a reasoning or synthesis layer; it doesn't learn or improve over queries

## Alternatives to Consider

For smaller, curated knowledge bases, a vector database may be overkill:
- **Full-text search** (BM25, Elasticsearch) works well for keyword-heavy domains
- **Compiled wiki** (see [[karpathy-llm-wiki]]) pre-synthesizes knowledge so retrieval is simpler
- **Graph databases** excel when relationships between entities matter more than semantic similarity

## Related pages

- [[rag]]
- [[karpathy-llm-wiki]]
