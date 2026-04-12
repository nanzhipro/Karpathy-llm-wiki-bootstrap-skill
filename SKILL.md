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

# LLM Wiki Bootstrap

Scaffold a complete LLM Wiki framework from zero: directory structure, schema (the AI instruction file), index, log, and operational workflows. The output is a ready-to-use wiki that the user and their LLM agent can immediately start populating.

**Guiding principle**: The wiki is a persistent, compounding artifact. The LLM writes and maintains all content. The human curates sources, directs analysis, and asks questions. This skill builds the scaffolding; the user and their LLM fill it with knowledge over time.

## Workflow

Six sequential phases. Each phase completes before the next begins.

### Phase 1: Gather Requirements

**BLOCKING** — must complete before any file creation.

Use `AskUserQuestion` with ALL questions in ONE call:

**Q1** — header: "Domain", question: "What is this wiki about?"

- Options: "Research topic", "Book / media", "Personal (goals, health, learning)", "Business / team", "Other (describe below)"

**Q2** — header: "Wiki name", question: "Short name for the wiki root directory?"

- Free text. Suggest: `{domain}-wiki` (e.g. `ml-wiki`, `lotr-wiki`, `health-wiki`)

**Q3** — header: "Agent", question: "Which LLM agent will maintain this wiki?"

- Options:
  - "Claude Code" — generates `CLAUDE.md`
  - "OpenAI Codex" — generates `AGENTS.md`
  - "Copilot (VS Code)" — generates `.github/copilot-instructions.md`
  - "Other / generic" — generates `SCHEMA.md`

**Q4** — header: "Editor", question: "Primary editor for browsing the wiki?"

- Options: "Obsidian (recommended)", "VS Code", "Other / plain files"

**Q5** — header: "Source types", question: "What kind of sources will you add?"

- Multi-select. Options: "Web articles", "PDFs / papers", "Books (chapter by chapter)", "Meeting notes / transcripts", "Personal notes / journals", "Images / diagrams", "Data files (CSV, JSON)", "Other"

**Q6** — header: "Output location", question: "Where to create the wiki?"

- Options:
  - "Current directory" — create `{wiki-name}/` here
  - "Custom path" — user types absolute path

Store all answers as session variables for subsequent phases.

### Phase 2: Create Directory Structure

Based on Phase 1 answers, create the directory tree:

```
{wiki-root}/
├── raw/                    # Immutable source documents
│   └── assets/             # Images, attachments (if image sources selected)
├── wiki/                   # LLM-maintained markdown pages
│   ├── index.md            # Content catalog
│   ├── log.md              # Chronological operation log
│   └── overview.md         # High-level synthesis (starts empty)
├── {schema-file}           # AI instruction file (name from Q3)
└── .gitignore              # Ignore OS files, keep everything else
```

**Conditional directories**:

| Condition                   | Add                                                    |
| --------------------------- | ------------------------------------------------------ |
| Q5 includes images/diagrams | `raw/assets/`                                          |
| Q4 = Obsidian               | `.obsidian/` is NOT created (Obsidian auto-creates it) |
| Any source type selected    | `raw/` with a `.gitkeep`                               |

### Phase 3: Generate Schema File

The schema is the most critical output. It instructs the LLM agent how to operate on the wiki. Generate it using the template at [references/templates/schema.md](references/templates/schema.md).

**Customization rules**:

| Variable               | Source                                |
| ---------------------- | ------------------------------------- |
| `{WIKI_NAME}`          | Q2 answer                             |
| `{DOMAIN_DESCRIPTION}` | Q1 answer (expanded to 1-2 sentences) |
| `{SOURCE_TYPES}`       | Q5 answers, comma-separated           |
| `{SCHEMA_FILENAME}`    | Determined by Q3                      |
| `{EDITOR}`             | Q4 answer                             |
| `{DATE}`               | Current date in YYYY-MM-DD            |

After generating, adapt section details:

- If domain is "Book / media" → add character, timeline, and plot-thread page types
- If domain is "Research" → add paper-summary and claim-tracking page types
- If domain is "Personal" → add journal-entry and goal-tracking page types
- If domain is "Business / team" → add decision-log and meeting-summary page types

### Phase 4: Generate Initial Wiki Files

Create three seed files using templates in `references/templates/`:

**4.1 index.md** — from [references/templates/index.md](references/templates/index.md)

- Start with the universal sections (Sources, Entities, Concepts, Comparisons, Synthesis)
- Add domain-specific sections matching the page types injected in Phase 3:
  - Research → Papers, Claims, Methods, Datasets
  - Book / media → Characters, Timelines, Themes, Locations
  - Personal → Journal, Goals, Habits, Lessons
  - Business → Decision Logs, Meetings, Projects, Stakeholders
- Leave content sections empty with placeholder comments

**4.2 log.md** — from [references/templates/log.md](references/templates/log.md)

- Write the first entry: wiki creation event with current date

**4.3 overview.md** — from [references/templates/overview.md](references/templates/overview.md)

- Minimal stub that explains the wiki's purpose and domain

### Phase 5: Editor Configuration

**If Q4 = Obsidian**:

Do NOT create `.obsidian/` or modify Obsidian settings. Instead, append a `## Obsidian Setup` section to the schema file with recommendations:

- Set "Attachment folder path" to `raw/assets/` in Settings → Files and links
- Install recommended plugins: Dataview (frontmatter queries), Marp (slide decks if needed)
- Use graph view to inspect wiki structure
- Bind "Download attachments" hotkey if using Web Clipper

**If Q4 = VS Code**:

Create `.vscode/settings.json` with markdown-friendly defaults:

```json
{
  "files.exclude": { "**/.DS_Store": true },
  "editor.wordWrap": "on",
  "markdown.preview.breaks": true
}
```

Do NOT append an Obsidian section to the schema file.

**If Q4 = Other / plain files**:

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
