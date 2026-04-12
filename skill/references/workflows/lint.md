# Lint Workflow — Reference

Detailed protocol for wiki health checks. Catches structural issues, contradictions, and knowledge gaps.

## Trigger

User says "lint", "health check", "review wiki", "check consistency", or "wiki maintenance".

## Core Sequence

### Step 1: Full Scan

Read `wiki/index.md`, then read every page listed. Build an internal model of:
- All pages and their types
- All `[[wikilinks]]` and their targets
- All `sources` frontmatter entries
- All contradiction blocks
- All tags

### Step 2: Run Checks

Execute each check category. Collect findings as a numbered list.

**2.1 Structural Checks**

| Check | Issue | Severity |
|-------|-------|----------|
| Orphan pages | Page exists but no other page links to it | Medium |
| Broken wikilinks | `[[target]]` has no matching file | High |
| Missing pages | Concept/entity mentioned 3+ times across pages but has no dedicated page | Medium |
| Index drift | Page exists in `wiki/` but not listed in `index.md` | High |
| Empty pages | Page has frontmatter but no meaningful body content | Low |

**2.2 Content Checks**

| Check | Issue | Severity |
|-------|-------|----------|
| Unresolved contradictions | Contradiction block with `Resolution: pending` older than 2 ingests | Medium |
| Stale claims | Page claims X, but a newer source (by date) contradicts it without the page being updated | High |
| Single-source concepts | Concept page backed by only 1 source | Low |
| Outdated overview | `wiki/overview.md` not updated since 3+ ingests ago | Medium |

**2.3 Cross-reference Checks**

| Check | Issue | Severity |
|-------|-------|----------|
| Missing backlinks | Page A links to Page B, but B doesn't link back to A (when topically relevant) | Low |
| Isolated clusters | Groups of pages that link to each other but not to the rest of the wiki | Medium |
| Tag inconsistency | Same concept tagged differently across pages | Low |

### Step 3: Report

Output findings grouped by severity:

```markdown
## Wiki Lint Report — {date}

### High ({count})
1. {finding with specific file references}
...

### Medium ({count})
1. ...

### Low ({count})
1. ...

### Suggestions
- {Proactive recommendations: topics to explore, sources to find, pages to create}
```

### Step 4: Triage

Ask user: "Which items should I fix now?" Accept:
- "All" — fix everything
- "High only" — fix high-severity items
- Specific numbers — "Fix 1, 3, 7"
- "None" — just log the report

### Step 5: Execute Fixes

For each approved fix:
- Create/update pages as needed
- Update cross-references
- Resolve contradictions if newer data is clear
- Update `wiki/index.md`

### Step 6: Log

Append to `wiki/log.md`:
```
## [{date}] lint
- Total issues: {count}
- High: {count}, Medium: {count}, Low: {count}
- Fixed: {list of fixed items}
- Deferred: {list of deferred items}
```

## Suggested Lint Schedule

| Wiki Size | Recommended Frequency |
|-----------|-----------------------|
| < 10 sources | After every 3 ingests |
| 10-50 sources | Weekly or after every 5 ingests |
| 50+ sources | After every 10 ingests, or when queries return inconsistent results |
