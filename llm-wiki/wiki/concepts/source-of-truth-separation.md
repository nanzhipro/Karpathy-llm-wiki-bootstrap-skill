---
title: Source-of-Truth Separation
type: concept
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [architecture, raw-sources, derivation, discipline]
---

# Source-of-Truth Separation

The source insists on a strict division between immutable raw inputs and editable derived knowledge [LLM Wiki](../sources/llm-wiki-pattern.md). Raw documents remain the source of truth in `raw/`, while the LLM is allowed to freely rewrite and reorganize the derived pages in `wiki/`. `(high confidence)`

## Why This Matters

- It preserves evidence integrity: source materials are never silently rewritten.
- It allows the wiki to evolve aggressively without losing provenance.
- It makes the knowledge base regenerable: if needed, derived pages can be revised or rebuilt from stable inputs.

## Architectural Form

| Layer | Mutability | Owner |
|------|------------|-------|
| `raw/` | Immutable | Human-curated evidence layer |
| `wiki/` | Mutable | LLM-maintained synthesis layer |
| schema file | Mutable, but controlled | Human and LLM co-evolve operating rules |

This is one of the core architectural commitments inside [[llm-wiki]].
