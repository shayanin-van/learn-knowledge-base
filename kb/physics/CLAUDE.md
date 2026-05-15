# Physics Knowledge Base

A curated knowledge base of physics concept pages for Thai high school students (ม.4–ม.6), maintained by Claude Code.

## Purpose

This knowledge base is the **physics layer** of an AI companion that helps Thai students with homework and exam preparation. Content must be trustworthy, pedagogically sound, and aligned with the Thai national curriculum (IPST/สสวท.).

## Folder structure

```
raw/          -- source documents (immutable — never modify these)
wiki/         -- concept pages maintained by Claude
wiki/index.md -- table of contents for all physics concept pages
wiki/log.md   -- append-only record of all operations
```

## Ingest workflow

When the user adds a new source to `raw/` and asks you to ingest it:

1. Read the full source document
2. Discuss key takeaways with the user before writing anything
3. Create or update concept pages in `wiki/`
4. Update `wiki/index.md` and append to `wiki/log.md`

## Concept page format

Every concept page follows this structure:

````markdown
# [Concept Name] — [Thai Name]

**Summary**: One to two sentences.

**Curriculum anchor**: [Company curriculum unit ref]

**Level**: [e.g. ม.4]

**Prerequisites**: [[concept-a]], [[concept-b]]

**Sources**: (source: filename) — note `(IPST — authoritative)` where applicable

**Last updated**: YYYY-MM-DD

---

[Free-form content body — author decides the structure appropriate for this concept.
May include: definitions, intuition, derivations, common misconceptions,
pedagogical framing, teaching notes, IPST conflict notes inline where they appear, etc.]

<!-- Simulation block — OPTIONAL, only include if a simulation exists -->
## Simulation

<!-- Phase 1 (testing): iframe embed -->
<iframe src="https://..." width="800" height="600" allowfullscreen></iframe>

<!-- Phase 2+ (production): replace iframe with this JSON block -->
<!--
```simulation
{
  "id": "concept-simulation-id",
  "params": { "key": "value" }
}
```
-->

*Description of what the simulation demonstrates.*

## Related pages

- [[related-concept]]
- [[worked-example-concept-name]]
````

## Page naming

- Lowercase with hyphens: `simple-harmonic-motion.md`
- One concept per file, one depth level per file
- Worked examples on separate pages, named `worked-example-[concept].md`

## Math notation

LaTeX standard — inline `$...$` and block `$$...$$`.

## Source types

Source files in `raw/` are prefixed with a type tag. Each type has a specific role during ingestion:

**`[IPST-Textbook]`** — The Thai national curriculum textbook (IPST/สสวท.).
- Primary factual reference. Any claim that contradicts this must be noted explicitly inline.
- This is the conflict-resolution authority: when sources disagree, IPST wins.
- Cite as: `(source: [IPST-Textbook]-filename — authoritative)`

**`[IPST-Teacher-Manual]`** — The IPST teacher handbook.
- Use to identify minimum learning objectives for the topic.
- Primary source for common student misconceptions — these should be surfaced explicitly in concept pages.
- Good source for pedagogical approach ideas.
- Cite as: `(source: [IPST-Teacher-Manual]-filename)`

**`[OE-Textbook]`** — Our company's student-facing textbook. Problem-set heavy, aligned with university admission exams (O-NET/PAT/TCAS).
- May contain topics beyond the IPST minimum standard. When this happens, those advanced topics must be included in the knowledge base — our students need them.
- Flag advanced topics explicitly in concept pages so the AI companion can calibrate accordingly.
- Cite as: `(source: [OE-Textbook]-filename)`

**`[OE-S-Map]`** — Our company's curriculum summary (mindmap-style).
- Good starting point for thinking about wiki page structure and scope.
- Treat as a structural guide, not a content source — always adjust granularity based on pedagogical needs.
- Cite as: `(source: [OE-S-Map]-filename)`

**`[International-Textbook]`** — Established international undergraduate textbook.
- Authoritative factual reference, especially for derivations and deeper theory.
- Content may be too advanced for direct use — extract only what is relevant and accessible at high school level.
- Useful for verifying facts and providing depth behind simplifications.
- Cite as: `(source: [International-Textbook]-filename)`

## Source authority

IPST/สสวท. is the conflict-resolution authority. When sources present conflicting facts, IPST takes precedence. Note conflicts inline in the content body where they arise — not in the page header.

## Citation rules

- Every factual claim should reference its source: `(source: filename)`
- If two sources disagree, note the contradiction explicitly
- If a claim has no source, mark it: `(source: needs verification)`

## Rules

- Never modify anything in `raw/`
- Always update `wiki/index.md` and `wiki/log.md` after any change
- Keep page names lowercase with hyphens
- Write in clear, plain language suitable for high school students
