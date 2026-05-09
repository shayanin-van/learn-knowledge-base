# Project Context

**Summary**: Overview of the academic knowledge base project — the company, target users, and what we are building and why.

**Sources**: Discussion with project owner (2026-05-09).

**Last updated**: 2026-05-09

---

## Company & Platform

An EdTech company in Thailand building an online learning platform for Thai students. The platform focuses on helping students achieve their academic goals. Approximately 70% of current users are high school students preparing for university admission exams (O-NET, PAT, TCAS). The remaining users are lower secondary and primary school students preparing for school exams.

## The Knowledge Base Project

The project is to build an **AI companion** that can answer student questions while they study — for homework help and exam preparation. The knowledge base is the foundation that the AI system is built on.

The AI companion must be:
- **Highly trustworthy** — incorrect answers actively harm students before exams
- **Expert in explanation** — not just factually correct but pedagogically sound
- **Mastery-oriented** — designed to help students genuinely understand, not just get answers

The project owner's responsibility is the knowledge base layer. A separate development team builds the application and platform on top of it.

## Language & Curriculum

- **Primary language**: Thai
- **Authoritative curriculum standard**: IPST / สสวท. (Thai national curriculum)
- **International sources**: Included as enrichment — most academic knowledge is international and the goal is the richest possible knowledge base. IPST takes precedence when sources conflict.

## Content Scope

**Phase 1 focus**: High school level (ม.4–ม.6)
- Mathematics
- Physics
- Biology
- Chemistry

Broader scope (future phases) will expand to other subjects and grade levels.

## Source Material

- Textbooks (including IPST textbooks — access confirmed)
- Past exam papers (O-NET, PAT)
- International academic sources
- Interactive simulations (planned — to be coded separately and linked to concepts)

## Pedagogy

The AI companion should support configurable teaching styles per student. The project owner personally favors Socratic teaching (guiding students to discover answers), but the system should be flexible enough to also provide direct explanations when appropriate. The knowledge base needs to be rich enough to support both modes.

## Related pages

- [[karpathy-llm-wiki]]
- [[rag]]
- [[vector-database]]
- [[knowledge-base-architecture]]
