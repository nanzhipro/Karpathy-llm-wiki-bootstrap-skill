# Ingest Workflow — Reference

Detailed protocol for ingesting a new source into the wiki. The schema file contains a condensed version; this document covers edge cases and rationale.

## Trigger

User adds a file to `raw/` and instructs the LLM to process it. Trigger phrases: "ingest", "process", "add this source", "read this and update the wiki".

## Pre-flight Checks

1. Verify file exists in `raw/`
2. Determine file type:
   | Type | Action |
   |------|--------|
   | Markdown / text | Read directly |
   | PDF | Extract text (ask user for tool if unavailable) |
   | Image | Describe contents, note as visual source |
   | CSV / JSON | Summarize structure and key data points |
   | Other | Ask user how to handle |
3. Check `wiki/log.md` — has this source been ingested before? If yes, ask user: "This source was ingested on {date}. Re-ingest (update) or skip?"

## Core Sequence

### Step 1: Read and Comprehend

Read the entire source. Produce internal notes (not written to file):
- **Primary language** of the source (e.g. Chinese, English, Japanese). All wiki pages created or updated from this source MUST be written in this language. Structural elements (YAML keys, `type` values, filenames, table column headers, section headings in index/log) stay in English.
- 3-5 key claims or data points
- Named entities (people, orgs, products, places)
- Concepts and frameworks introduced
- Anything that contradicts or extends existing wiki knowledge

### Step 2: Discuss with User

Present 2-3 bullet summary of key takeaways. Ask:
- "Anything I should emphasize or de-emphasize?"
- "Any corrections to my reading?"

This step is skippable if user says "ingest silently" or "batch ingest".

### Step 3: Create Source Summary Page

Write `wiki/sources/{slug}.md`:

```yaml
---
title: "{source title}"
type: source-summary
created: {date}
updated: {date}
sources: [{filename}]
tags: [{auto-generated tags}]
---
```

Note: `sources` uses filenames only (e.g. `[article.md]`), not full paths like `raw/article.md`.

Body structure:
1. **Summary** — 2-3 paragraph overview
2. **Key Claims** — numbered list with inline quotes
3. **Entities Mentioned** — list with wikilinks
4. **Concepts** — list with wikilinks
5. **Notable Quotes** — verbatim with page/section references
6. **Limitations / Bias** — note if single-perspective, dated, etc.

### Step 4: Ripple Updates

For each entity and concept the source touches:
- If page exists → add new information, update `sources` frontmatter, revise summary if needed
- If page doesn't exist → create it with information from this source
- If source contradicts existing content → add contradiction block to both pages

**Contradiction format**:
```markdown
> ⚠️ CONTRADICTION (added {date})
> {Source A} claims: "{quote or paraphrase}"
> {Source B} claims: "{quote or paraphrase}"
> Resolution: {pending | Source B supersedes (newer data) | both valid in different contexts}
```

### Step 5: Update Index

Add source entry to `wiki/index.md` Sources table. Add/update entity and concept entries.

### Step 6: Update Log

Append structured entry to `wiki/log.md`.

### Step 7: Revise Overview

Re-read `wiki/overview.md` and assess whether the new source changes the big picture. If yes, revise. If no, skip. Even for small wikis, keep the overview current — a single source can reshape the narrative.

## Batch Ingest

When processing multiple sources at once:
1. Process sequentially (not in parallel — each source may affect the next)
2. Skip Step 2 (user discussion) unless contradictions are found
3. Consolidate log entries into a single batch entry
4. Revise overview once at the end, not after each source

## Edge Cases

| Situation | Resolution |
|-----------|------------|
| Source overlaps significantly with existing source | Note overlap, only add genuinely new information |
| Source is a primary document (e.g., legal text, spec) | Summarize but also link to raw file for exact wording |
| Source is in a different language than existing wiki pages | Write this source's wiki pages in the source's own language. For cross-source pages that mix languages, use the majority language or ask the user. |
| Source contains images | Describe key images in text, reference image files in `raw/assets/` |
| Source is very long (book chapter, 50+ pages) | Break into sections, create one summary page with section headers |
