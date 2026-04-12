---
title: LLM Wiki
type: concept
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [llm-wiki, knowledge-base, agents, markdown]
---

# LLM Wiki

An LLM Wiki is a persistent, interlinked markdown knowledge base maintained by an LLM rather than manually authored page by page [LLM Wiki](../sources/llm-wiki-pattern.md). The central idea is that the LLM incrementally compiles knowledge into a durable artifact and keeps it current, instead of reconstructing answers from raw documents every time a question is asked. `(high confidence)`

## Defining Properties

- It sits between the user and the raw source corpus.
- It is persistent: pages accumulate, cross-references persist, and synthesis compounds.
- It is editable by the LLM as the primary maintainer of the `wiki/` layer.
- It is governed by an explicit schema file that defines ingest, query, and lint behavior.

## Minimal Architecture

The pattern depends on three layers:

1. Immutable raw sources
2. LLM-maintained wiki pages
3. A schema file that constrains the LLM's behavior

That separation is formalized in [[source-of-truth-separation]].

## Why It Differs From Generic Chat Workflows

The wiki is not just storage for notes. It is a continuously updated knowledge artifact that preserves links, contradictions, comparisons, and synthesis between sources. That makes it closer to a maintained codebase than to a chat log, and it explains why the source compares Obsidian to an IDE and the LLM to a programmer [LLM Wiki](../sources/llm-wiki-pattern.md).

## Related Pages

- [[rag]]
- [[rag-vs-llm-wiki]]
- [[wiki-operations-loop]]
- [[human-ai-maintenance-split]]
- [[why-llm-wikis-work]]
