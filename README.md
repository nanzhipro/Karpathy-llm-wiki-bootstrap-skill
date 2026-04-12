# LLM Wiki Bootstrap

Turn any topic into a structured, LLM-maintained markdown wiki — the "Karpathy pattern" of compounding knowledge through human curation + AI maintenance.

## How It Works

```
You (human)                          LLM Agent
───────────                          ─────────
Drop sources into raw/       ──►     Reads source
Ask "ingest raw/foo.md"      ──►     Extracts entities, concepts, claims
                                     Creates/updates wiki pages
                                     Updates index + log
Ask questions                ──►     Reads index → relevant pages → answers
Say "lint"                   ──►     Checks consistency, flags issues
```

The wiki has three layers:

| Layer | Location | Owner | Mutable? |
|-------|----------|-------|----------|
| Sources | `raw/` | Human | Read-only (immutable after adding) |
| Wiki pages | `wiki/` | LLM | Yes — created, updated, deleted by agent |
| Schema | AI instruction file (root) | Human + Skill | Rarely changed after bootstrap |

The **schema file** is the key artifact. It tells the LLM agent exactly how to structure pages, handle ingestion, answer queries, and audit the wiki. The skill generates it automatically based on your domain and toolchain choices.

## Quick Start

1. **Trigger the skill** — tell your LLM agent:
   > "bootstrap a wiki" / "create wiki" / "new wiki" / "llm wiki"

2. **Answer 6 questions** — domain, name, agent type, editor, source types, output location

3. **Open the generated directory** in your editor. You'll see:
   ```
   {wiki-name}/
   ├── raw/                 # Drop source files here
   ├── wiki/
   │   ├── index.md         # Content catalog (LLM reads this first)
   │   ├── log.md           # Chronological operation log
   │   └── overview.md      # High-level synthesis
   ├── {schema-file}        # AI instruction file
   └── .gitignore
   ```

4. **Add your first source** to `raw/` and tell the agent:
   > "read {schema-file}, then ingest raw/{filename}"

5. **Repeat**. Each source compounds the wiki.

## Three Core Operations

| Operation | Trigger | What Happens |
|-----------|---------|-------------|
| **Ingest** | "ingest raw/{file}" | Source → summary + entities + concepts + index update + log entry |
| **Query** | Ask any domain question | Agent reads index → relevant pages → synthesized answer with citations |
| **Lint** | "lint" / "health check" | Detects contradictions, orphan pages, missing links, stale claims |

Detailed protocols for each operation are embedded in the generated schema file. The agent follows them automatically.

## Schema File Variants

The skill picks the right filename based on your LLM agent:

| Agent | Schema File | Why |
|-------|------------|-----|
| Claude Code | `CLAUDE.md` | Claude's project-level instruction convention |
| OpenAI Codex | `AGENTS.md` | Codex's agent instruction convention |
| Copilot (VS Code) | `.github/copilot-instructions.md` | Copilot's custom instruction path |
| Other / generic | `SCHEMA.md` | Universal fallback |

All variants contain the same content — only the filename differs.

## Domain Customization

The skill adds domain-specific page types based on your choice:

| Domain | Extra Page Types |
|--------|-----------------|
| Research | Paper summary, Claim, Method, Dataset |
| Book / Media | Character, Timeline, Plot thread, Theme, Location |
| Personal | Journal entry, Goal, Habit tracker, Lesson |
| Business / Team | Decision log, Meeting summary, Project, Stakeholder |

These supplement the universal types (Source summary, Entity, Concept, Comparison, Synthesis, Overview) that every wiki gets.

## Important Notes

### The Human's Job
- **Curate sources** — the LLM only knows what you feed it. Quality in → quality out.
- **Direct analysis** — ask questions, request comparisons, point out gaps.
- **Review takeaways** — during ingest, the agent presents key claims for your confirmation before filing.

### The LLM's Job
- **Maintain all wiki pages** — create, update, cross-link, flag contradictions.
- **Never modify `raw/`** — sources are immutable records.
- **Keep index and log current** — every operation is logged and cataloged.

### Things to Know
- **Contradictions are features, not bugs.** When sources disagree, the agent flags both positions with `⚠️ CONTRADICTION` markers rather than silently picking a winner.
- **The wiki is regenerable.** If wiki pages get corrupted, delete `wiki/` and re-ingest all sources from `raw/`. The raw layer is the durable truth.
- **Git works out of the box.** Everything is plain text. Commit early, commit often.
- **Overview is auto-managed.** After each ingest, the agent reviews `wiki/overview.md` and revises it if the new source changes the big picture.
- **One concept per page.** If a page tries to cover two distinct ideas, split it. This keeps cross-linking precise.

### Obsidian Users
If you chose Obsidian as your editor:
- Set **Attachment folder path** → `raw/assets/` (Settings → Files and links)
- Install **Dataview** plugin for frontmatter queries (e.g., list all entities from a given source)
- Use **Graph view** to spot orphan pages and hub nodes
- The skill does NOT touch `.obsidian/` — your config stays yours

## File Reference

| File | Purpose |
|------|---------|
| `SKILL.md` | Agent-executable skill definition (the bootstrap protocol) |
| `references/templates/schema.md` | Template for the AI instruction file |
| `references/templates/index.md` | Template for wiki index |
| `references/templates/log.md` | Template for operation log |
| `references/templates/overview.md` | Template for overview page |
| `references/templates/domain-page-types.md` | Domain-specific page type snippets |
| `references/templates/gitignore.md` | Git ignore template |
| `references/workflows/ingest.md` | Full ingest protocol with edge cases |
| `references/workflows/query.md` | Query and answer-filing protocol |
| `references/workflows/lint.md` | Wiki health-check protocol |

