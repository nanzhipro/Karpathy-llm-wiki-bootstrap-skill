<p align="right">
  <a href="./README.zh-CN.md">简体中文</a>
</p>

# LLM Wiki Bootstrap

An installable skill plus a working reference implementation for building persistent, LLM-maintained Markdown wikis.

This repository is meant to be explained publicly in two layers:

- `skill/` is the distributable package you install and use to generate your own wiki
- `llm-wiki/` is a real example wiki created from this skill and then maintained through the ingest/query/lint loop

## Quick Install (Recommended)

```bash
npx skills add nanzhipro/Karpathy-llm-wiki-bootstrap-skill@llm-wiki-bootstrap
```

For a non-interactive user-level install:

```bash
npx skills add nanzhipro/Karpathy-llm-wiki-bootstrap-skill@llm-wiki-bootstrap -g -y
```

## What This Repository Ships

### 1. The Installable Skill

The skill lives in [skill](./skill) and is the only source of truth for distribution.

It scaffolds a new wiki by generating:

- a raw source layer
- a maintained `wiki/` layer
- a schema file such as `AGENTS.md`, `CLAUDE.md`, or `SCHEMA.md`
- the operating conventions for ingest, query, and lint

### 2. The Reference Implementation

[llm-wiki](./llm-wiki) is not a placeholder. It is a working example produced from this skill and then actually run as a living wiki.

That means it shows the full pattern in compiled form:

- raw source files in [llm-wiki/raw](./llm-wiki/raw)
- an agent contract in [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md)
- maintained wiki pages in [llm-wiki/wiki](./llm-wiki/wiki)

For external readers, the clean framing is:

- install `skill/` if you want to create your own wiki
- inspect `llm-wiki/` if you want to see what the system looks like after it has been bootstrapped and used for real

## Why This Pattern Exists

Most document workflows with LLMs look like RAG: upload files, retrieve chunks at question time, and synthesize an answer from scratch each time. That works, but it does not accumulate structure.

This skill packages a different model:

- raw sources remain immutable
- the agent incrementally compiles knowledge into a wiki
- the wiki becomes a persistent artifact that gets richer over time
- useful answers can be filed back into the wiki instead of disappearing into chat history

## System Model

There are really four layers when you present this repository end to end:

| Layer | Location | Role |
| --- | --- | --- |
| Skill package | `skill/` | Reusable bootstrap logic and templates |
| Raw sources | `raw/` | Immutable evidence layer |
| Schema | `AGENTS.md` / `CLAUDE.md` / `SCHEMA.md` | Agent operating contract |
| Wiki pages | `wiki/` | Maintained knowledge layer |

The skill package creates the bottom three layers inside a user wiki. The reference implementation in `llm-wiki/` shows the result after that process has already happened.

## Reference Example: `llm-wiki/`

The example wiki currently includes:

```text
llm-wiki/
├── AGENTS.md
├── raw/
│   ├── Karpathy x.md
│   └── llm-wiki-pattern.md
└── wiki/
    ├── index.md
    ├── log.md
    ├── overview.md
    ├── concepts/
    ├── entities/
    ├── comparisons/
    ├── sources/
    └── synthesis/
```

Useful entry points:

- [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md) shows the generated operating contract
- [llm-wiki/wiki/index.md](./llm-wiki/wiki/index.md) shows the catalog the agent navigates through
- [llm-wiki/wiki/log.md](./llm-wiki/wiki/log.md) shows the chronological operation history
- [llm-wiki/wiki/overview.md](./llm-wiki/wiki/overview.md) shows the current top-level synthesis

The canonical idea note behind this example lives at the repository root in [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md).

## Source Grounding And Origin

The reference wiki is grounded in Karpathy's original llm-wiki idea. The canonical source text for that idea is [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) at the repository root.

In other words, the example is not just a random demo corpus. It is a reference implementation whose raw layer is organized from that original concept and then extended with additional sources so readers can see how the skill turns an idea into a maintained knowledge base.

In the current example corpus, the raw layer includes:

- [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md), the canonical original idea note
- [llm-wiki/raw/llm-wiki-pattern.md](./llm-wiki/raw/llm-wiki-pattern.md), the example-local raw source derived from that original pattern
- [llm-wiki/raw/Karpathy x.md](./llm-wiki/raw/Karpathy%20x.md), which shows how new sources get ingested into the same evolving wiki

For public release, the right explanation is:

- the skill encodes the method
- the example wiki demonstrates the method in use
- the raw corpus begins from [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) and then expands with additional sources

## Recommended Deployment Layout

Use `.agent/skills/` as the canonical install location for the skill itself. If Claude, Codex, or another runtime expects a separate discovery directory, point that runtime back to the same installed copy with a symbolic link instead of duplicating files.

```text
.agent/
└── skills/
    └── llm-wiki-bootstrap/
        ├── SKILL.md
        └── references/
```

Example symlinks:

```bash
ln -s /absolute/path/to/.agent/skills/llm-wiki-bootstrap ~/.claude/skills/llm-wiki-bootstrap
ln -s /absolute/path/to/.agent/skills/llm-wiki-bootstrap ~/.codex/skills/llm-wiki-bootstrap
```

The important principle is to keep one real installed copy and link every runtime back to it.

## What The Skill Generates

When someone installs the skill and creates a new wiki, the generated structure looks like this:

```text
{wiki-name}/
├── raw/
├── wiki/
│   ├── index.md
│   ├── log.md
│   └── overview.md
├── {schema-file}
└── .gitignore
```

## Core Operations

| Operation | Trigger | Result |
| --- | --- | --- |
| Ingest | `"ingest raw/{file}"` | Converts a source into summaries, entities, concepts, links, index updates, and a log entry |
| Query | Ask a domain question | Reads the index, opens relevant pages, and answers with citations |
| Lint | `"lint"` / `"health check"` | Audits contradictions, stale claims, orphan pages, and missing links |

## Schema File Variants

| Agent | Schema File |
| --- | --- |
| Claude Code | `CLAUDE.md` |
| OpenAI Codex | `AGENTS.md` |
| Copilot (VS Code) | `.github/copilot-instructions.md` |
| Other / generic | `SCHEMA.md` |

The operating model stays the same. Only the filename changes to match the agent's conventions.

## Repository Layout

| Path | Purpose |
| --- | --- |
| [skill/SKILL.md](./skill/SKILL.md) | Installable skill definition |
| [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) | Canonical original idea note behind the example corpus |
| [skill/references/templates](./skill/references/templates) | Templates used during bootstrap |
| [skill/references/workflows](./skill/references/workflows) | Detailed ingest, query, and lint workflow references |
| [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md) | Generated agent instructions for the example wiki |
| [llm-wiki/raw](./llm-wiki/raw) | Example source corpus |
| [llm-wiki/wiki](./llm-wiki/wiki) | Example compiled wiki output |

## Public Positioning

If you are describing this project externally, the clean one-line summary is:

> `LLM Wiki Bootstrap` is an installable skill for creating persistent LLM-maintained Markdown wikis, bundled with a real `llm-wiki/` reference implementation that shows the pattern running on a source corpus grounded in [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md).
