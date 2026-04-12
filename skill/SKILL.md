---
name: llm-wiki-bootstrap
description: >
  Scaffolds a personal LLM Wiki from scratch — the Karpathy pattern of incrementally
  building a persistent, interlinked markdown knowledge base maintained by LLMs.
  Generates directory structure, schema file, index, log, and workflow conventions.
  Use when user says "create wiki", "new wiki", "bootstrap wiki", "llm wiki",
  "knowledge base", "start a wiki", "build a wiki", or wants to set up a
  structured markdown knowledge base for any domain.
version: 0.1.0
---

# Karpathy LLM Wiki Bootstrap

Scaffold a complete LLM Wiki framework from zero: directory structure, schema (the AI instruction file), index, log, and operational workflows. The output is a ready-to-use wiki that the user and their LLM agent can immediately start populating.

**Guiding principle**: The wiki is a persistent, compounding artifact. The LLM writes and maintains all content. The human curates sources, directs analysis, and asks questions. This skill builds the scaffolding; the user and their LLM fill it with knowledge over time.

## Workflow

Six sequential phases. Each phase completes before the next begins.

### Phase 1: Gather Requirements

**BLOCKING** — must complete before any file creation.

Use a **Codex-first intake**. When the host exposes structured input UI, use it. Otherwise, ask the same questions as concise plain-text follow-ups. Do NOT promise an interactive selection UI in every runtime.

**Round 1 — structured UI**

Ask exactly 3 single-choice questions:

**Domain** — "What kind of wiki are we bootstrapping?"

- "Research topic (Recommended)" — academic, technical, or investigation-heavy subjects
- "Book / media" — novels, films, shows, biographies, or narrative corpora
- "Personal or team" — personal systems, learning, health, goals, projects, or business/team knowledge

**Agent** — "Which LLM agent will maintain this wiki?"

- "OpenAI Codex (Recommended)" — defaults to `AGENTS.md`
- "Claude Code" — defaults to `CLAUDE.md`
- "Copilot / other" — requires a plain-text follow-up in Round 3 to choose between `.github/copilot-instructions.md` and `SCHEMA.md`

**Editor** — "Primary editor for browsing the wiki?"

- "Obsidian (Recommended)"
- "VS Code"
- "Other / plain files"

**Round 2 — structured UI**

Ask exactly 2 single-choice questions:

**Source profile** — "What kind of source mix will this wiki ingest most often?"

- "Text-first (Recommended)" — web articles, PDFs / papers, books, meeting notes / transcripts, personal notes / journals
- "Mixed media" — Text-first plus images / diagrams and data files
- "Custom" — requires a plain-text follow-up in Round 3 with the exact source types

**Output location** — "Where should the wiki be created?"

- "Current directory (Recommended)" — create `{wiki-name}/` in the current working directory
- "Custom absolute path" — requires a plain-text follow-up in Round 3 with the absolute parent directory

**Round 3 — plain-text follow-up**

Ask concise plain-text follow-ups:

- Always ask for `Wiki name`. Suggest `{domain}-wiki` (e.g. `ml-wiki`, `lotr-wiki`, `health-wiki`).
- If `Output location = Custom absolute path`, ask for the **absolute parent directory**. Build the final wiki root as `{custom-parent}/{wiki-name}/`.
- If `Source profile = Custom`, ask for the exact source types as a comma-separated list.
- If `Agent = Copilot / other`, ask whether to generate `.github/copilot-instructions.md` or `SCHEMA.md`.
- If `Domain = Personal or team`, ask whether to normalize the wiki as `Personal` or `Business / team`.

Normalize and store these values for subsequent phases:

| Normalized value | Source |
| ---------------- | ------ |
| `domain-type` | Round 1 `Domain`, plus Round 3 clarification for `Personal or team` |
| `wiki-name` | Round 3 `Wiki name` |
| `editor` | Round 1 `Editor` |
| `schema-file` | Round 1 `Agent`, plus Round 3 clarification for `Copilot / other` |
| `source-types` | `Source profile` mapping below, or the Round 3 custom list |
| `wiki-root` | `wiki-name` + output mode (`{cwd}/{wiki-name}` or `{custom-parent}/{wiki-name}`) |

Use this explicit `Source profile` mapping:

| Source profile | Normalized `source-types` |
| -------------- | ------------------------- |
| `Text-first` | `Web articles`, `PDFs / papers`, `Books (chapter by chapter)`, `Meeting notes / transcripts`, `Personal notes / journals` |
| `Mixed media` | All `Text-first` sources plus `Images / diagrams`, `Data files (CSV, JSON)` |
| `Custom` | User-provided exact list from the Round 3 plain-text follow-up |

### Phase 2: Create Directory Structure

Based on the normalized Phase 1 values, create the directory tree:

```
{wiki-root}/
├── raw/                    # Immutable source documents
│   └── assets/             # Images, attachments (if image sources selected)
├── wiki/                   # LLM-maintained markdown pages
│   ├── index.md            # Content catalog
│   ├── log.md              # Chronological operation log
│   └── overview.md         # High-level synthesis (starts empty)
├── {schema-file}           # AI instruction file (name from normalized agent choice)
└── .gitignore              # Ignore OS files, keep everything else
```

**Conditional directories**:

| Condition | Add |
| --------- | --- |
| Normalized `source-types` include `Images / diagrams` | `raw/assets/` |
| `editor = Obsidian` | `.obsidian/` is NOT created (Obsidian auto-creates it) |
| Normalized `source-types` are non-empty | `raw/` with a `.gitkeep` |

### Phase 3: Generate Schema File

The schema is the most critical output. It instructs the LLM agent how to operate on the wiki. Generate it using the template at [references/templates/schema.md](references/templates/schema.md).

**Customization rules**:

| Variable               | Source                                |
| ---------------------- | ------------------------------------- |
| `{WIKI_NAME}`          | Normalized `wiki-name`                |
| `{DOMAIN_DESCRIPTION}` | Normalized `domain-type` (expanded to 1-2 sentences) |
| `{SOURCE_TYPES}`       | Normalized `source-types`, comma-separated |
| `{SCHEMA_FILENAME}`    | Normalized `schema-file`              |
| `{EDITOR}`             | Normalized `editor`                   |
| `{DATE}`               | Current date in YYYY-MM-DD            |

After generating, adapt section details:

- If `domain-type = Book / media` → add character, timeline, and plot-thread page types
- If `domain-type = Research` → add paper-summary and claim-tracking page types
- If `domain-type = Personal` → add journal-entry and goal-tracking page types
- If `domain-type = Business / team` → add decision-log and meeting-summary page types

### Phase 4: Generate Initial Wiki Files

Create three seed files using templates in `references/templates/`:

**4.1 index.md** — from [references/templates/index.md](references/templates/index.md)

- Start with the universal sections (Sources, Entities, Concepts, Comparisons, Synthesis)
- Add domain-specific sections matching the page types injected in Phase 3:
  - Research → Papers, Claims, Methods, Datasets
  - Book / media → Characters, Timelines, Themes, Locations
  - Personal → Journal, Goals, Habits, Lessons
  - Business / team → Decision Logs, Meetings, Projects, Stakeholders
- Leave content sections empty with placeholder comments

**4.2 log.md** — from [references/templates/log.md](references/templates/log.md)

- Write the first entry: wiki creation event with current date

**4.3 overview.md** — from [references/templates/overview.md](references/templates/overview.md)

- Minimal stub that explains the wiki's purpose and domain

### Phase 5: Editor Configuration

**If `editor = Obsidian`**:

Do NOT create `.obsidian/` or modify Obsidian settings. Instead, append a `## Obsidian Setup` section to the schema file with recommendations:

- Set "Attachment folder path" to `raw/assets/` in Settings → Files and links
- Install recommended plugins: Dataview (frontmatter queries), Marp (slide decks if needed)
- Use graph view to inspect wiki structure
- Bind "Download attachments" hotkey if using Web Clipper

**If `editor = VS Code`**:

Create `.vscode/settings.json` with markdown-friendly defaults:

```json
{
  "files.exclude": { "**/.DS_Store": true },
  "editor.wordWrap": "on",
  "markdown.preview.breaks": true
}
```

Do NOT append an Obsidian section to the schema file.

**If `editor = Other / plain files`**:

Skip editor configuration entirely. Do NOT append any editor-specific section to the schema file. Do NOT create `.vscode/`.

### Phase 6: Summary and Next Steps

After all files are created, output a brief summary:

```
Wiki scaffolded at {wiki-root}/

Structure:
  raw/          → Drop source documents here
  wiki/         → LLM-maintained pages (index, log, overview)
  {schema-file} → AI instructions for wiki operations

Next steps:
  1. Open {wiki-root}/ in {editor}
  2. Add your first source to raw/
  3. Tell your LLM agent: "Read {schema-file}, then ingest raw/{filename}"
```

## Post-Bootstrap Operations

The schema file generated in Phase 3 defines three core operations. These are documented in detail in:

- [references/workflows/ingest.md](references/workflows/ingest.md) — source ingestion protocol
- [references/workflows/query.md](references/workflows/query.md) — query and answer-filing protocol
- [references/workflows/lint.md](references/workflows/lint.md) — wiki health-check protocol

The schema file embeds condensed versions of these workflows. The reference files here contain the full rationale and edge cases for skill maintainers.

## Design Principles

These inform all generated content:

1. **LLM-native** — all instructions are written as executable protocols for language models, not prose for humans
2. **Source of truth separation** — raw sources are immutable; the wiki is derived and regenerable
3. **Incremental compilation** — each source is integrated once; knowledge compounds, not re-derived
4. **Convention over configuration** — sensible defaults, minimal required decisions
5. **Editor-agnostic core** — markdown files work everywhere; editor-specific features are optional layers
6. **Git-friendly** — plain text, no binary blobs in wiki/, version history for free
