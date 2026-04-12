---
title: RAG vs LLM Wiki
type: comparison
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [rag, llm-wiki, comparison, retrieval]
---

# RAG vs LLM Wiki

This comparison captures the first source's central contrast: both patterns help an LLM work with documents, but they place synthesis at different points in the workflow [LLM Wiki](../sources/llm-wiki-pattern.md). `(high confidence)`

| Dimension | RAG | LLM Wiki |
|------|------|------|
| Primary moment of synthesis | Query time | Ingest time, then continuously maintained |
| Durable artifact | Usually limited | Central: the wiki itself compounds |
| Cross-reference maintenance | Often implicit or absent | Explicit, maintained across pages |
| Contradiction handling | Rediscovered per query | Can be flagged once and preserved |
| Best fit | Fast retrieval over document collections | Long-horizon cumulative understanding |

## Interpretation

The source is not arguing that RAG is useless. It argues that standard RAG alone is insufficient when the goal is cumulative understanding across many interactions. The LLM Wiki pattern keeps more of the intermediate structure that would otherwise disappear after each answer.

## Related Pages

- [[rag]]
- [[llm-wiki]]
- [[why-llm-wikis-work]]
