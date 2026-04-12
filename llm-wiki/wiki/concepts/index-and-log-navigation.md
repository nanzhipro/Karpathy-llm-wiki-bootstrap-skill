---
title: Index and Log Navigation
type: concept
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [navigation, indexing, logs, retrieval]
---

# Index and Log Navigation

The source distinguishes two lightweight navigation primitives for the wiki: `index.md` for content-oriented discovery and `log.md` for chronological memory [LLM Wiki](../sources/llm-wiki-pattern.md). Instead of relying immediately on embedding infrastructure, the pattern starts with explicit markdown navigation and only adds stronger search tools later if scale demands it. `(high confidence)`

## Roles

| File | Function |
|------|----------|
| `index.md` | Catalog of pages, summaries, and categories |
| `log.md` | Append-only timeline of ingests, queries, and lint passes |

## Design Consequences

- The LLM should read `index.md` first when answering questions.
- `log.md` provides recency awareness and operational memory.
- Both files are small but structurally important because they make the wiki inspectable by humans and parsable by tools.

This concept is part of [[llm-wiki]] and becomes more important as the system matures beyond a handful of sources.
