---
title: LLM Wiki
type: source-summary
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [llm-wiki, knowledge-base, agents, rag, obsidian]
---

# LLM Wiki

> Source file: `raw/llm-wiki-pattern.md`

## One-Paragraph Summary

This source proposes replacing query-time rediscovery with a persistent, incrementally maintained wiki that sits between raw documents and the user. Instead of re-synthesizing knowledge from scratch on every question, the LLM compiles knowledge once into linked markdown pages and keeps that artifact current as new sources arrive. The human curates sources and directs inquiry; the LLM performs the maintenance work that usually makes wikis collapse under their own bookkeeping burden.

## Key Claims

1. Standard RAG is useful, but it tends to rediscover relevant knowledge from scratch on each query rather than accumulating durable synthesis.
2. A wiki maintained by the LLM can act as a persistent, compounding knowledge artifact, with contradictions, links, and summaries already integrated.
3. The architecture should cleanly separate immutable raw sources from LLM-authored derived pages, with a schema file governing behavior.
4. The operating loop is not just ingest; it also includes query-time answer filing and periodic linting to keep the knowledge base healthy.
5. Editor and tooling choices such as Obsidian, qmd, Marp, and Dataview are optional layers around an editor-agnostic markdown core.

## Core Structure

| Layer | Role | Notes |
|------|------|------|
| `raw/` | Immutable source-of-truth layer | Human-curated, never rewritten by the LLM |
| `wiki/` | Derived knowledge layer | Summaries, concept pages, comparisons, synthesis |
| schema file | Behavioral contract | Tells the LLM how to ingest, answer, and lint |

This source makes the separation between raw evidence and derived synthesis explicit, which is central to [[source-of-truth-separation]] and [[llm-wiki]].

## Operational Model

| Operation | What happens | Why it matters |
|------|------|------|
| Ingest | Read new source, summarize it, update related pages, log changes | Knowledge compounds at write time |
| Query | Read existing wiki pages, synthesize an answer, optionally file it back | Exploration itself becomes durable knowledge |
| Lint | Search for contradictions, gaps, stale claims, orphans | Keeps the wiki healthy over time |

This source treats that loop as the practical mechanism behind [[wiki-operations-loop]].

## Important Comparisons

- The main foil is [[rag]]: both approaches retrieve from documents, but the LLM Wiki pattern pushes synthesis and maintenance forward into the ingest process rather than delaying everything until query time.
- The document also frames the wiki as a better long-term fit for cumulative work such as research, book reading, internal team memory, and due diligence than a chat-history-only workflow.

## Mentioned Tools and Historical Lineage

- **Obsidian** is framed as the browsing environment, with the LLM acting as the "programmer" editing the wiki.
- **qmd** is suggested as an optional search layer when the wiki grows beyond what `index.md` can comfortably support.
- **Marp** and **Dataview** are positioned as optional tooling for presentations and structured queries.
- The historical analogue is **Memex**, used here as a lineage reference for associative, curated knowledge stores.

## What This Source Adds To The Wiki

- A first-principles definition of [[llm-wiki]]
- A critique baseline in [[rag-vs-llm-wiki]]
- A design rule in [[source-of-truth-separation]]
- An operating loop in [[wiki-operations-loop]]
- A maintenance thesis in [[why-llm-wikis-work]]

## Open Questions Raised By The Source

- At what scale does `index.md` stop being enough without dedicated search?
- How much ingest should remain human-supervised versus fully automated?
- What is the best way to represent contradictions when many sources disagree?
- Which page types become most valuable once the wiki reaches dozens or hundreds of sources?
