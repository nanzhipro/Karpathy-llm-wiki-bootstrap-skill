# Query Workflow — Reference

Detailed protocol for answering questions against the wiki and optionally filing answers as new pages.

## Trigger

User asks a question about the wiki's domain. No special command needed — any question triggers this workflow.

## Core Sequence

### Step 1: Navigate via Index

Read `wiki/index.md`. Identify all pages potentially relevant to the query. If the wiki has a search tool configured, use it instead.

### Step 2: Read Relevant Pages

Read identified pages. If a page references other pages that seem relevant, follow those links too. Stop when you have sufficient context or have read 15+ pages (at which point, work with what you have).

### Step 3: Synthesize Answer

Compose an answer that:
- Directly addresses the user's question
- Cites wiki pages inline: `[Page Title](wiki/path/to/page.md)`
- Notes confidence level: high (multiple corroborating sources), medium (single source), low (inference)
- Flags if the wiki lacks sufficient information to fully answer

### Step 4: File Decision

After answering, assess: is this answer a valuable artifact worth preserving?

**File-worthy signals**:
- Comparison or analysis the user is likely to revisit
- New connection discovered between existing concepts
- Synthesis across 3+ sources
- Answer fills a gap in the wiki's coverage

**Not file-worthy**:
- Simple factual lookups
- Clarifying questions about wiki structure
- Trivial or one-off queries

If file-worthy, ask user: "This analysis seems worth keeping. File as `wiki/{type}/{slug}.md`?"

If user agrees:
1. Create the page with proper frontmatter
2. Add cross-references to/from related pages
3. Update `wiki/index.md`
4. Append to `wiki/log.md`:
   ```
   ## [{date}] query-filed | {title}
   - Question: {original question}
   - Filed as: wiki/{type}/{slug}.md
   - Related pages: {list}
   ```

## Answer Formats

Adapt output format to the question type:

| Question Type | Format |
|---------------|--------|
| "What is X?" | Prose explanation, 1-3 paragraphs |
| "Compare X and Y" | Markdown table with dimensions as rows |
| "What do we know about X?" | Structured summary with source citations |
| "What are the open questions on X?" | Numbered list with confidence assessments |
| "Summarize the state of X" | Section-based overview with key claims |
| "Timeline of X" | Chronological list or table |

## When Wiki Cannot Answer

If the wiki lacks information:
1. State clearly what's missing
2. Suggest sources that might fill the gap (web search, specific documents)
3. Optionally: create a stub page noting the knowledge gap
   ```yaml
   ---
   title: "{topic}"
   type: concept
   created: {date}
   updated: {date}
   sources: []
   tags: [stub, needs-sources]
   ---
   ```
