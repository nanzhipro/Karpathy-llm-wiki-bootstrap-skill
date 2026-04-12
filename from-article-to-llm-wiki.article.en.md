# Reading Is Not Enough: How to Compile an Article into an LLM Wiki That Remembers It for You

We have all been there: an article feels revelatory the moment we finish it, yet a week later only a few hazy impressions remain. A book still makes sense when we close it, but a month on, all we keep are scattered fragments.

The problem is rarely comprehension in the moment. It is what happens next—turning that comprehension into a structure you can maintain over time. In the long run, the hard part is not "reading it again" but keeping links updated, revising judgments, absorbing new material, and preserving context.

**Most personal knowledge systems decay not because the content is unworthy, but because the maintenance cost is too high.**

What makes Karpathy's [original concept note](./karpathy-llm-wiki-original.md) worth revisiting is not just "let an LLM take notes for you." It goes one step further: **stop treating AI as a one-shot answering machine and start treating it as a long-running editor for your knowledge base.**

The [llm-wiki](./llm-wiki) example in this repo is a complete run of that idea. It is not a set of placeholder pages. It is real source material incrementally compiled into a living wiki—complete with an [index](./llm-wiki/wiki/index.md), a [log](./llm-wiki/wiki/log.md), an [overview](./llm-wiki/wiki/overview.md), concept pages, comparison pages, and synthesis pages.

What is worth showing the world is not "it can generate a table of contents," but "it can turn a piece of source material into a maintainable understanding structure."

---

## The Problem Is Not Retrieval—It Is Accumulation

> RAG is like ad-hoc retrieval; an LLM Wiki is like incremental compilation. The former is good at finding material; the latter is good at retaining understanding.

Most LLM document workflows today are essentially RAG: hand files to a model, retrieve a few chunks at query time, and stitch together an answer on the spot. That works—especially for quick lookups and one-off questions.

But it has a hidden cost: every session rediscovers the same relevant fragments, re-stitches the same cross-document connections, and potentially re-derives the same conclusions. Nothing truly settles. Knowledge flickers in a chat window and vanishes.

The [RAG vs. LLM Wiki comparison page](./llm-wiki/wiki/comparisons/rag-vs-llm-wiki.md) in this repo makes the difference precise: RAG synthesizes at query time; an LLM Wiki pulls synthesis forward to ingest time and keeps maintaining it afterward.

This is why **an LLM Wiki resembles a codebase more than a clipboard**:

- Raw sources live in `raw/`, immutable
- LLM-maintained pages live in `wiki/`, continuously rewritten, restructured, and cross-linked
- A schema file like [AGENTS.md](./llm-wiki/AGENTS.md) acts as an operating contract—specifying how the model should ingest, query, and lint

Evidence layer: stable. Knowledge layer: evolvable. Behavior layer: constrained. Once these three are separated, the system can genuinely run long-term.

---

## What Does This Example Wiki Actually Prove?

If you only skim the repo description, it looks like "a skill that scaffolds a wiki." The real proof, however, lives in the [operation log](./llm-wiki/wiki/log.md).

### First Ingest: From Idea to Structure

The first ingest round starts from the [LLM Wiki source summary](./llm-wiki/wiki/sources/llm-wiki-pattern.md). It produces not just a summary page, but an entire web of interlinked pages:

- What is an [LLM Wiki](./llm-wiki/wiki/concepts/llm-wiki.md)
- Why [source-of-truth separation](./llm-wiki/wiki/concepts/source-of-truth-separation.md) matters
- Why maintenance is the make-or-break factor
- Why you need an [ingest-query-lint operations loop](./llm-wiki/wiki/concepts/wiki-operations-loop.md)
- How it differs from RAG

In other words, the original idea does not remain "I read an article." It gets decomposed into structure, process, comparison, and synthesis.

### Second Ingest: From Self-Definition to Absorbing External Material

The second round ingests the [Karpathy capability observations summary](./llm-wiki/wiki/sources/karpathy-x.md). This is the more important test, because it proves the wiki can do more than explain itself.

After the new material enters, the system spawns concept pages on [AI capability perception gap](./llm-wiki/wiki/concepts/ai-capability-perception-gap.md), [peaky capabilities](./llm-wiki/wiki/concepts/peaky-capabilities.md), and [verifiable rewards](./llm-wiki/wiki/concepts/verifiable-rewards.md), plus comparison and synthesis pages like [consumer AI vs. frontier agentic models](./llm-wiki/wiki/comparisons/consumer-ai-vs-frontier-agentic-models.md) and [why AI discourse fragments](./llm-wiki/wiki/synthesis/why-ai-discourse-fragments.md).

At this point the wiki is no longer just "explaining what an LLM Wiki is." It is using the same mechanism to absorb new observations, extend the boundary of its judgments, and weave two sources into one knowledge structure.

### The Overview Page: Where Two Threads Converge

This is where the [overview page](./llm-wiki/wiki/overview.md) earns its keep.

It does not simply place two source summaries side by side. Instead, it offers a higher-order synthesis: on one side, "why long-lived knowledge artifacts need LLM maintenance"; on the other, "why public perception of AI capability fractures."

These threads are not the same, yet they coexist in one wiki and illuminate each other through concept, comparison, and synthesis pages. **A living knowledge base proves its value not by how much material it stores, but by what reusable relationships it builds between them.**

---

## Why This Approach Matters for Learning

Many people equate "learning" with a single reading, a single summary, a single high-quality Q&A session. But if the goal is to remember and keep using a topic, what you actually need is to turn the act of reading into an artifact that can evolve.

The value of an LLM Wiki is that it produces exactly that artifact.

### 1. It Solves the Maintenance Problem

The repo's [Why LLM Wikis Work](./llm-wiki/wiki/synthesis/why-llm-wikis-work.md) page points to a plain truth: **people are perfectly capable of thinking—they just do not want to do bookkeeping forever.**

Summaries go stale. Links break. Old judgments get overturned by new material. Good analyses sink into chat history. LLMs happen to be well-suited to this kind of repetitive, granular editorial labor.

The human's role shifts from "manually maintaining every page" to "selecting material, setting direction, asking questions, reviewing key judgments." The model's role becomes "keeping the knowledge base running day to day." That is the division of labor described in [Human-AI Maintenance Split](./llm-wiki/wiki/concepts/human-ai-maintenance-split.md).

### 2. It Solves the Traceability Problem

Raw sources are never rewritten; they stay in `raw/`. Derived pages can be rewritten boldly, but every judgment traces back to a source summary, and when needed, back to the original text.

This [source-of-truth separation](./llm-wiki/wiki/concepts/source-of-truth-separation.md) matters because learning is not about stacking polished conclusions—it is about preserving the evidence chain for "why I arrived at this judgment."

Many AI note-taking systems eventually lose trust, not because summaries are poorly written, but because raw evidence and derived judgments get mixed together. Over time, nobody can tell which layer should be revised.

### 3. It Solves the "Learning Scatters Instead of Compounds" Problem

An LLM Wiki is not a static notebook. It is an operations loop. The [Wiki Operations Loop](./llm-wiki/wiki/concepts/wiki-operations-loop.md) page distills it into three actions:

- **ingest**: compile new material in
- **query**: ask questions against the existing structure and commit the results
- **lint**: periodically check for contradictions, orphan pages, and stale conclusions

Learning thus ceases to be a one-time input and becomes an ongoing cycle of compilation, use, and correction.

---

## From Articles to Papers, Reports, and Books

This example starts from a single concept note and a single observation post, but what it really aims to demonstrate is not "this method suits one Karpathy note." It is that **this method suits any material you intend to understand over the long term.**

- **For articles**: decompose core claims, key concepts, the author's judgments, and open questions into linkable pages—rather than settling for a single reading note.
- **For papers**: incrementally compile methods, hypotheses, metrics, experimental results, and related work into a stable structure that new papers can be ingested onto.
- **For research reports**: track entities, trend judgments, comparative relationships, and temporal changes so successive editions correct each other rather than merely pile up.
- **For books**: let chapters, characters, themes, argument threads, or case networks grow organically, so that "finished reading" does not mean "finished"—it means "now I have a companion knowledge base that can keep being questioned and revised."

Even with only two sources, this wiki has already naturally grown source summaries, concepts, comparisons, syntheses, an index, and a log—distinct page types at different levels of abstraction.

**What it demonstrates is not volume, but mechanism.** Once the mechanism is in place, what you add going forward is material, not methodology.

---

## What Matters Most When Presenting This Publicly

If you have to explain this project to the outside world, the key message is not "LLMs are great at summarizing," nor "this skill can initialize a directory structure."

A more accurate framing: **you can use the LLM Wiki build-and-compile approach to incrementally turn an article, paper, research report, or book into a traceable, maintainable, queryable knowledge artifact.**

It does not replace reading. It does not replace judgment. It replaces the maintenance labor most likely to cause a knowledge system to be abandoned halfway.

Nor does it mean RAG is obsolete. It means that for long-term learning, query-time retrieval alone is not enough. You also need a persistently growing middle layer that retains the structure already formed.

---

From this vantage point, the [llm-wiki](./llm-wiki) example is its own best proof: starting from the [original concept note](./karpathy-llm-wiki-original.md), after two ingest rounds it is no longer "a summary of one article" but a knowledge structure capable of absorbing new material, revising old judgments, and surfacing conceptual relationships.

What is truly worth remembering is not any single page—it is that the whole thing is now executable.

Keep raw material in the evidence layer, delegate understanding to the wiki layer, encode operating rules in the schema—and an LLM Wiki turns "I read it" into "it stays with me," and "I know a bit" into "I keep building on it."
