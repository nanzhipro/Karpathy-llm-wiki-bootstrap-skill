---
title: RAG
type: concept
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [rag, retrieval, baseline, comparison]
---

# RAG

In this wiki's first source, RAG appears as the baseline pattern for question answering over documents: upload files, retrieve relevant chunks at query time, then generate an answer [LLM Wiki](../sources/llm-wiki-pattern.md). The source does not reject RAG outright; instead, it argues that traditional RAG often fails to accumulate durable synthesis across repeated queries. `(high confidence)`

## What This Source Critiques

- Relevant fragments must often be rediscovered for each new question.
- Subtle cross-document synthesis must be rebuilt repeatedly.
- Contradictions and comparisons are not naturally preserved as first-class artifacts.

## Scope Note

This page currently reflects only how the first source frames RAG. It is not yet a full survey of RAG architectures, indexing methods, or retrieval strategies. `(single-source)`

## Related Pages

- [[llm-wiki]]
- [[rag-vs-llm-wiki]]
