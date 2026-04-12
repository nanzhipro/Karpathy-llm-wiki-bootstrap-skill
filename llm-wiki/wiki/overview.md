---
title: Overview
type: overview
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md, Karpathy x.md]
tags: []
---

# llm-wiki — Overview

> This page is the top-level synthesis of the entire knowledge base.
> It will be revised as sources are ingested and understanding deepens.

## Scope

LLM and AI systems research — exploring model behavior, agent workflows, RAG, evaluation, tooling, and knowledge-management patterns through articles, papers, and diagrams.

## Current State

Two sources have been ingested so far: [LLM Wiki](sources/llm-wiki-pattern.md) and [Karpathy x: AI Capability Perception Gap](sources/karpathy-x.md). The current picture is that durable knowledge artifacts and frontier agentic capability are reinforcing themes: AI systems are becoming more operationally useful in technical domains, and that makes structured long-horizon knowledge workflows more relevant.

## Key Themes

- [[llm-wiki]] as a persistent knowledge artifact rather than an ephemeral chat answer
- [[source-of-truth-separation]] between immutable evidence and mutable synthesis
- [[wiki-operations-loop]] as the mechanism that keeps knowledge compounding
- [[human-ai-maintenance-split]] as the social and operational design that makes the system sustainable
- [[rag-vs-llm-wiki]] as the current baseline comparison
- [[ai-capability-perception-gap]] as a discourse problem caused by sampling different model tiers and domains
- [[peaky-capabilities]] as a current shape-of-progress claim
- [[verifiable-rewards]] as one explanation for why technical domains are moving faster

## Emerging Product Map

- [[openai-codex]] and [[claude-code]] currently anchor the wiki's notion of frontier agentic technical systems
- [[advanced-voice-mode]] currently anchors the contrastive consumer-facing experience discussed in source two

## Working Thesis

Two working theses currently coexist:

1. The knowledge-management bottleneck is not reading, but maintenance. If an LLM can reliably absorb bookkeeping work such as summarization, cross-linking, contradiction tracking, and page upkeep, then a personal or team wiki can remain alive instead of decaying after initial enthusiasm.
2. Public conversations about AI capability are currently distorted because people generalize from very different products, access tiers, and task domains.

## Open Questions

- When does a markdown-first index stop being enough without dedicated search?
- What review loop is needed to keep automated wiki maintenance trustworthy?
- Which page types become most useful as the source corpus grows?
- How should contradictions be represented when many sources disagree over time?
- Which technical gains will transfer outward into consumer-facing product surfaces?
- How should this wiki distinguish durable product facts from fast-moving source impressions?
