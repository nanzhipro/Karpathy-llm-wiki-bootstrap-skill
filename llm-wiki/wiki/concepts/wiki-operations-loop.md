---
title: Wiki Operations Loop
type: concept
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [workflow, ingest, query, lint, operations]
---

# Wiki Operations Loop

The source defines the wiki as an operational loop rather than a static repository: ingest new material, answer questions against the current wiki, and periodically lint the result for contradictions and gaps [LLM Wiki](../sources/llm-wiki-pattern.md). `(high confidence)`

## Three Core Operations

1. **Ingest**: read a new source, summarize it, update related pages, refresh the index, and append to the log
2. **Query**: synthesize answers from existing wiki pages and optionally file valuable outputs back into the wiki
3. **Lint**: inspect the wiki for contradictions, stale claims, orphan pages, missing pages, and weak coverage

## Why The Loop Matters

The loop turns the wiki into a living system. New sources add structure, queries create reusable knowledge, and linting counteracts drift and fragmentation. This gives the pattern operational continuity rather than leaving it as a one-time note-taking setup.

## Related Pages

- [[llm-wiki]]
- [[index-and-log-navigation]]
- [[human-ai-maintenance-split]]
