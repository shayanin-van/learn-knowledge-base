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

## Master record — tagging, not structure

The master record is a **tagging system**. It is not a table of contents and its lesson/subtopic rows are not a blueprint for wiki pages. Always derive page structure from the sources and pedagogical needs. Apply master record tags to pages after their structure is decided — a page may cover multiple subtopic rows, or a subtopic may warrant splitting across finer pages.

## Concept page format

Every concept page follows this structure:

````markdown
# [Concept Name] — [Thai Name]

**Summary**: One to two sentences.

**Curriculum anchor**: [Company curriculum unit ref]

**Level**: [e.g. มัธยมปลาย — use the Grade label from the master record]

**Prerequisites**: [[concept-a]], [[concept-b]]

**Sources**: (source: filename) — note `(IPST — authoritative)` where applicable

**Last updated**: YYYY-MM-DD

---

[Free-form content body — author decides the structure appropriate for this concept.
May include: definitions, intuition, derivations, common misconceptions,
pedagogical framing, teaching notes, IPST conflict notes inline where they appear, etc.]

**Pedagogical principle — "what it feels like" first**: Where appropriate, open the content body with physical intuition before introducing formulas or derivations. Anchor to a concrete, tangible example first (e.g. a spring-cart, a pendulum, a real-world object). Build the mathematical framework as a consequence of that intuition — not as the opening statement. This principle applies whenever a concept has a natural physical image that students can connect to. It does not apply to purely mathematical or reference pages (e.g. equation summary pages).

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

## Language and tone

Write like **ติวเตอร์ที่สอนเก่งมาก** — a skilled private tutor explaining to a student, not a formal textbook. Concretely:

- **Avoid academic register words** that students don't use in conversation. Banned words and replacements:
  - `อนุมาน` → `หา`, `สร้างสมการ`, `ขั้นตอน`
  - `ลักษณะเฉพาะ (defining characteristic)` → `สมบัติที่โดดเด่นที่สุด`
  - `เปรียบเทียบกับสมการ...` → `เมื่อเทียบกัน:`
  - `ปรากฏการณ์นี้เกิดขึ้นเพราะ` → `เหตุผลคือ`
  - `ข้อควรระวัง` → `จุดที่สับสนบ่อย`
  - `หมายเหตุ` → use a plain conversational sentence instead

- **Don't over-focus on experiments.** Lab activities and experimental protocols are for school classrooms. Our pages are tutor-style — skip procedural experiment descriptions unless needed to build intuition.

- **Use active, direct sentences.** Prefer "กฎข้อสองบอกว่า..." over "จากกฎการเคลื่อนที่ข้อสองของนิวตัน...". Imagine saying it out loud to a student.

- **Misconception tables** use the ❌/✅ format (two columns: wrong belief on the left, correct understanding on the right).

## Rules

- Never modify anything in `raw/`
- Always update `wiki/index.md` and `wiki/log.md` after any change
- Keep page names lowercase with hyphens
- **Write all concept page content in Thai**, with English technical terms introduced in parentheses on first use, e.g., "การสั่นพ้อง (resonance)". After the first introduction, use the Thai term. The only other exceptions are LaTeX math, source filenames in citations, and wiki-link targets (page names stay lowercase with hyphens).
