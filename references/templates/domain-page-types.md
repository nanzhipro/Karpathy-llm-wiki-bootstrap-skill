# Domain-Specific Page Types

Snippets to inject into the schema's Page Types table based on the user's domain selection.

## Book / Media

```markdown
| Character | `wiki/characters/{name}.md` | Character profile: traits, arc, relationships, key scenes. |
| Timeline | `wiki/timeline/{event}.md` | Chronological event with date/chapter, participants, consequences. |
| Plot thread | `wiki/threads/{name}.md` | Narrative thread tracking: introduction, development, resolution. |
| Theme | `wiki/themes/{name}.md` | Thematic analysis with supporting quotes and scene references. |
| Location | `wiki/locations/{name}.md` | Setting description, significance, scenes set here. |
```

## Research

```markdown
| Paper summary | `wiki/papers/{slug}.md` | Structured summary: problem, method, results, limitations, citations. |
| Claim | `wiki/claims/{slug}.md` | Specific factual claim with supporting/contradicting evidence across sources. |
| Method | `wiki/methods/{name}.md` | Research method or technique: description, when to use, papers that employ it. |
| Dataset | `wiki/datasets/{name}.md` | Dataset profile: size, format, source, papers that use it, known limitations. |
```

## Personal

```markdown
| Journal entry | `wiki/journal/{date}.md` | Daily/weekly reflection: events, insights, mood, connections to goals. |
| Goal | `wiki/goals/{name}.md` | Goal definition, milestones, progress tracking, related journal entries. |
| Habit | `wiki/habits/{name}.md` | Habit tracking: trigger, routine, reward, streaks, adjustments. |
| Lesson | `wiki/lessons/{slug}.md` | Distilled insight from experience: context, lesson, application. |
```

## Business / Team

```markdown
| Decision log | `wiki/decisions/{slug}.md` | Decision record: context, options considered, choice made, rationale, outcome. |
| Meeting summary | `wiki/meetings/{date}-{topic}.md` | Meeting notes: attendees, agenda, decisions, action items. |
| Project | `wiki/projects/{name}.md` | Project overview: goals, status, key decisions, stakeholders. |
| Stakeholder | `wiki/stakeholders/{name}.md` | Stakeholder profile: role, interests, communication preferences. |
```
