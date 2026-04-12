---
title: Human-AI Maintenance Split
type: concept
created: 2026-04-12
updated: 2026-04-12
sources: [llm-wiki-pattern.md]
tags: [workflow, maintenance, collaboration, agents]
---

# Human-AI Maintenance Split

The source defines a deliberate division of labor: humans curate sources, choose directions, and ask good questions, while the LLM handles summarization, cross-linking, filing, and consistency work [LLM Wiki](../sources/llm-wiki-pattern.md). This is presented as a core reason the system remains sustainable over time rather than degrading into an abandoned wiki. `(high confidence)`

## Why It Matters

- Humans are best at choosing what is worth paying attention to.
- LLMs are well suited to repetitive maintenance across many pages.
- The design reduces the main failure mode of personal knowledge systems: bookkeeping grows faster than perceived value.

## Practical Implication

This split means the user should not try to hand-maintain every page. Instead, the human steers and reviews, while the LLM continuously updates the derived knowledge layer in `wiki/`. This idea supports [[llm-wiki]] and the ongoing [[wiki-operations-loop]].
