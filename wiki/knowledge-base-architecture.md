# Knowledge Base Architecture

**Summary**: The chosen architecture for the academic knowledge base — a hybrid of curated wiki-style knowledge pages with RAG retrieval on top.

**Sources**: Discussion with project owner (2026-05-09), [[karpathy-llm-wiki]], [[rag]], [[vector-database]].

**Last updated**: 2026-05-09

---

## Decision: Hybrid — Curated Knowledge Layer + RAG Retrieval

The architecture is a **hybrid**: a curated, concept-organized knowledge layer (LLM Wiki style) as the foundation, with vector-based RAG retrieval operating on top of that curated layer rather than raw source documents.

## Why Not Pure RAG

Pure RAG over raw source documents (textbooks, PDFs) is insufficient for this use case:

- **Chunking loses context**: Mathematical derivations and scientific explanations depend on surrounding context. Naive chunking breaks this.
- **Retrieval is imprecise**: A student's question may not lexically match the relevant passage. Retrieval failures produce wrong or irrelevant answers.
- **Hallucination risk**: When retrieval misses, the LLM fills gaps from its weights. For exam-level math and science, this is dangerous.
- **No quality hierarchy**: Raw RAG treats an IPST textbook and a random international source as equally authoritative. The system needs IPST to win when sources conflict.

## Why Not Pure LLM Wiki

A static compiled Markdown wiki alone is also insufficient:

- **Scale**: A full Thai high school curriculum across four subjects (math, physics, biology, chemistry) involves thousands of concepts. Each needing manually maintained pages is a large ongoing editorial burden.
- **Source diversity**: Thai + international sources, textbooks + past exams + simulations — a pure wiki struggles to represent this richness without becoming unwieldy.
- **Retrieval**: A flat wiki has no semantic search. The AI system needs to find the right concept pages given a free-form student question.

## The Hybrid Architecture

```
Raw Sources (immutable)
    ↓  [ingest + curate]
Curated Concept Pages (wiki layer)
    ↓  [embed + index]
Vector Index
    ↑  [semantic search]
AI System query → retrieved pages → grounded response
```

### Layer 1: Curated Concept Pages

The core quality layer. Each page covers a specific curriculum concept and follows a structured format:

- **Curriculum anchor**: tied to an IPST topic and grade level (e.g., "กฎของนิวตัน — ม.4")
- **Source citations**: every factual claim cites its source; IPST sources marked as authoritative
- **Multi-level explanation**: enough depth that the AI can explain simply or in detail
- **Cross-links**: connected to prerequisite concepts and related concepts
- **Simulation links**: pointers to interactive simulations when available (future phase)

This layer is what gives the system its trustworthiness. The quality comes from curation, not retrieval.

### Layer 2: RAG over Curated Pages

Vector embeddings of the curated pages (not raw PDFs) enable semantic search. When a student asks a question, the AI retrieves the most relevant concept pages and grounds its response in them.

Operating RAG on curated pages rather than raw documents means:
- Retrieved chunks are already high-quality and source-verified
- Context is coherent (pages are written to be self-contained)
- The LLM has less opportunity to hallucinate

## Source Authority Hierarchy

When IPST and international sources conflict, IPST takes precedence for the Thai curriculum context. International sources serve as enrichment — additional perspectives, deeper explanations, practice problems — not as overrides.

This hierarchy must be encoded in the metadata of each knowledge page so the AI system can surface it when needed.

## Pedagogy Support

The knowledge base does not encode teaching style directly. Instead, pages are written to be rich enough that the AI system layer can choose to:
- Provide a direct explanation
- Ask guiding Socratic questions
- Offer a worked example
- Link to an interactive simulation

The teaching style is configurable at the AI application layer (the dev team's responsibility), not baked into the knowledge base.

## Open Questions

- What embedding model handles Thai text well for the vector index? (multilingual-e5, OpenAI text-embedding-3, etc.)
- How to structure concept pages for math — formulas, LaTeX, step-by-step derivations?
- How will interactive simulations be linked — embedded metadata, separate index, or structured page section?
- What is the update/maintenance workflow as new sources are added or curriculum changes?

## Related pages

- [[project-context]]
- [[karpathy-llm-wiki]]
- [[rag]]
- [[vector-database]]
