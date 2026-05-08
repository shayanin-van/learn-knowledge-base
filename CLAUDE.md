# LLM Wiki

A personal knowledge base maintained by Claude Code.
Based on Andrej Karpathy's LLM Wiki pattern.

## Purpose

This wiki is a structured, interlinked knowledge base for exploring/bouncing ideas about the user's new project that aim to create an academic knowledge base (yes, we're doing a knowledge base about knowledge base, a knowledge base-ception, if you will!).

Claude maintains the wiki. The human curates sources, asks questions, and guides the analysis.

## Folder structure

```
raw/                          -- source documents (immutable -- never modify these)
wiki/                         -- markdown pages maintained by Claude
wiki/index.md                 -- table of contents for the entire wiki
wiki/log.md                   -- append-only record of all operations
progress/                     -- project progress tracking
progress/README.md            -- boss-facing dashboard, always current
progress/roadmap.md           -- planned work, prioritized
progress/ideas.md             -- ideas and key decisions log (append-only)
progress/sessions/YYYY-MM-DD.md -- per-session log
```

## Ingest workflow

When the user adds a new source to `raw/` and asks you to ingest it:

1. Read the full source document
2. Discuss key takeaways with the user before writing anything
3. Create a summary page in `wiki/` named after the source
4. Create or update concept pages for each major idea or entity
5. Add wiki-links ([[page-name]]) to connect related pages
6. Update `wiki/index.md` with new pages and one-line descriptions
7. Append an entry to `wiki/log.md` with the date, source name, and what changed

A single source may touch 10-15 wiki pages. That is normal.

## Page format

Every wiki page should follow this structure:

```markdown
# Page Title

**Summary**: One to two sentences describing this page.

**Sources**: List of raw source files this page draws from.

**Last updated**: Date of most recent update.

---

Main content goes here. Use clear headings and short paragraphs.

Link to related concepts using [[wiki-links]] throughout the text.

## Related pages

- [[related-concept-1]]
- [[related-concept-2]]
```

## Citation rules

- Every factual claim should reference its source file
- Use the format (source: filename.pdf) after the claim
- If two sources disagree, note the contradiction explicitly
- If a claim has no source, mark it as needing verification

## Question answering

When the user asks a question:

1. Read `wiki/index.md` first to find relevant pages
2. Read those pages and synthesize an answer
3. Cite specific wiki pages in your response
4. If the answer is not in the wiki, say so clearly
5. If the answer is valuable, offer to save it as a new wiki page

Good answers should be filed back into the wiki so they compound over time.

## Lint

When the user asks you to lint or audit the wiki:

- Check for contradictions between pages
- Find orphan pages (no inbound links from other pages)
- Identify concepts mentioned in pages that lack their own page
- Flag claims that may be outdated based on newer sources
- Check that all pages follow the page format above
- Report findings as a numbered list with suggested fixes

## Progress tracking

At the end of every session (or when significant progress is made):

1. Create or append to `progress/sessions/YYYY-MM-DD.md` — what was discussed, decisions made, open questions
2. Update `progress/README.md` — keep the dashboard current (status, recent wins, up next, open questions)
3. Update `progress/roadmap.md` — tick off completed items, add newly identified work
4. Append to `progress/ideas.md` — record any new ideas or decisions with their rationale

The progress files are designed to be machine-readable for a future boss-briefing chatbot. Keep entries structured and self-contained.

## Rules

- Never modify anything in the `raw/` folder
- Always update `wiki/index.md` and `wiki/log.md` after wiki changes
- Always update `progress/` files at the end of each session
- Keep page names lowercase with hyphens (e.g. `machine-learning.md`)
- Write in clear, plain language
- When uncertain about how to categorize something, ask the user
