# Source Types

**Summary**: The taxonomy of source file types used in this knowledge base, their roles, and how to treat each during ingestion.

**Sources**: Project owner definition (2026-05-15).

**Last updated**: 2026-05-15

---

Source files in `raw/` use a `[TYPE]` prefix in the filename. The type determines how the source is used when writing concept pages.

## Type reference

### `[IPST-Textbook]`
The Thai national curriculum textbook published by IPST/สสวท.

- **Role**: Primary factual reference and conflict-resolution authority
- **When sources conflict**: IPST wins. Note the conflict inline in the concept page where it arises.
- **What to extract**: Definitions, formulas, standard derivations, scope of content
- **Citation**: `(source: [IPST-Textbook]-filename — authoritative)`

---

### `[IPST-Teacher-Manual]`
The IPST teacher handbook accompanying the textbook.

- **Role**: Learning objectives + misconceptions + pedagogy
- **What to extract**:
  - Minimum learning objectives for the topic (what students are expected to know)
  - Common student misconceptions — surface these explicitly in concept pages
  - Suggested pedagogical approaches — use as inspiration for how to frame explanations
- **Citation**: `(source: [IPST-Teacher-Manual]-filename)`

---

### `[OE-Textbook]`
Our company's student-facing textbook. Problem-set heavy, aligned with O-NET/PAT/TCAS university admission exams.

- **Role**: Scope extension + exam-aligned problems
- **What to extract**:
  - Topics covered beyond the IPST minimum standard — these must be included; our students need them for exams
  - Flag advanced topics explicitly in concept pages
- **Citation**: `(source: [OE-Textbook]-filename)`

---

### `[OE-S-Map]`
Our company's curriculum summary in a mindmap-style format.

- **Role**: Structural reference — a starting point for wiki page organization
- **What to extract**: Topic scope and groupings to inform how to structure pages
- **Important**: Treat as a guide, not a constraint. Always adjust page granularity based on pedagogical needs — concept pages will often be finer-grained than the S-Map.
- **Citation**: `(source: [OE-S-Map]-filename)`

---

### `[International-Textbook]`
An established international undergraduate-level textbook (e.g., Halliday, Serway, Young).

- **Role**: Deep factual reference and derivation verification
- **What to extract**: Rigorous definitions, full derivations, depth behind high school simplifications
- **Important**: Content is often too advanced for direct use. Extract only what is relevant and accessible at high school level. Note where a concept is a simplification of a more complex treatment.
- **Citation**: `(source: [International-Textbook]-filename)`

---

## Ingest order recommendation

When all source types are available for a topic, ingest in this order:

1. `[OE-S-Map]` — understand the scope and structure first
2. `[IPST-Textbook]` — establish the authoritative factual baseline
3. `[IPST-Teacher-Manual]` — layer in objectives, misconceptions, pedagogy
4. `[OE-Textbook]` — identify scope extensions and exam-level topics
5. `[International-Textbook]` — add depth and verify derivations

## Related pages

- [[master-record]]
