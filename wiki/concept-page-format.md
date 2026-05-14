# Concept Page Format

**Summary**: The standard format for all knowledge base concept pages — a fixed metadata shell with a free-form content body.

**Sources**: Design discussion with project owner (2026-05-14).

**Last updated**: 2026-05-14

---

## Design Principles

- **One concept, one depth level, one page.** If a concept is taught differently at ม.4 vs ม.6, those are separate pages.
- **Free-form content body.** The metadata shell is fixed; the content structure is chosen by the author to best serve that specific concept. Different topics warrant different approaches — misconceptions, intuition-building, derivations, historical context, pedagogical framing, etc.
- **More than academic facts.** Pages may include preferred pedagogical explanations, common student confusions, and teaching notes that differentiate the AI companion from generic online sources.
- **LaTeX for math.** All formulas and derivations use LaTeX — inline `$...$` and block `$$...$$`. This is the platform standard.
- **IPST as conflict-resolution authority only.** IPST chapter structure does not dictate page organization. IPST value is: when sources present conflicting academic facts, IPST takes precedence. Conflicts and any notable alternative facts should be noted inline in the content body where they arise.
- **Worked examples on separate pages** (current default — subject to revision). A concept page focuses on the concept itself; example pages are linked from Related pages.

---

## Page Template

```markdown
# [Concept Name] — [Thai Name]

**Summary**: One to two sentences.

**Curriculum anchor**: [Company curriculum unit ref — provided per chapter]

**Level**: [e.g. ม.4]

**Prerequisites**: [[concept-a]], [[concept-b]]

**Sources**: (source: filename) — note `(IPST — authoritative)` where applicable

**Last updated**: YYYY-MM-DD

---

[Free-form content body — author decides the structure appropriate for this concept.
May include any of: definitions, intuition, derivations, common misconceptions,
pedagogical framing, teaching notes, IPST conflict notes inline where they appear, etc.]

<!-- Simulation block is OPTIONAL — only include if a simulation exists for this concept -->
## Simulation

<!-- MVP: URL reference with state parameters -->
[simulation-id]: https://platform.url/sim/concept-name?state=default

*Description of what the simulation demonstrates and relevant initial states.*

## Related pages

- [[related-concept]]
- [[worked-example-concept-name]]
```

---

## Notes on Specific Fields

### Curriculum anchor
Filled in per chapter as the project owner provides the company curriculum structure. Not derived from IPST chapter numbering.

### Prerequisites
Explicit wiki-links to concepts the student must understand before this page. This creates a concept dependency graph that the AI application layer can use to suggest prerequisite review.

### Sources
List all sources that informed the page. Mark IPST sources as authoritative. If an international source presents a conflicting fact that was overridden by IPST, note the conflict inline in the content body at the point where it arises — not in the header.

### Simulation
**Optional** — only included when a simulation exists for this concept. Omit the section entirely if there is no simulation.

For the current MVP phase, a simple URL reference with optional state query parameters (e.g., `?state=amplitude`). The parameter-based scheme is intentional: it is designed to support stateful simulation linking in a future phase, where the simulation initial state depends on the problem context the student is working on. Do not use bare links.

### Free-form content body
No fixed sections are required. Choose the structure that best serves the concept. Examples of what may appear (not a checklist):
- Conceptual definition and intuition
- Key formulas with variable explanations
- Step-by-step derivation
- Common student misconceptions and how to address them
- Preferred pedagogical framing
- Comparison to related or easily confused concepts
- IPST conflict notes (inline, where the conflict occurs)
- Teaching notes for the AI (e.g., "students often confuse this with X because...")

---

## Related pages

- [[knowledge-base-architecture]]
- [[project-context]]
