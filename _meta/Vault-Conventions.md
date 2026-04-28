---
type: meta
description: How this vault is organized — read once, then internalize
---

# Vault Conventions

Single source of truth for how files are organized, named, and tagged. The wrap-up and insight-extraction skills both read this file.

## Folder roles

| Folder | Holds | Lifecycle |
|---|---|---|
| `00_Inbox/` | Untriaged captures | Drain to zero regularly |
| `10_Conversations/` | Standalone AI chats | Append-only |
| `20_Projects/` | Multi-session project work | Active → Archive when done |
| `30_Code_Sessions/` | Standalone coding sessions | Append-only |
| `40_Insights/` | Atomic, durable knowledge | Append; rarely delete |
| `50_References/` | External docs/links | Append; prune stale |
| `60_MOCs/` | Hand-curated entry points | Edit as topics evolve |
| `90_Archive/` | Inactive projects | Move things in; rarely take out |

`_meta/` and `_templates/` are infrastructure — not content.

## File naming

- **Session/conversation notes**: `YYYY-MM-DD_short-slug.md`
- **Insight notes**: `kebab-case-sentence-fragment.md` (e.g., `prefer-async-over-sync-meetings.md`)
- **Reference notes**: `kebab-case-slug.md`
- **MOCs**: `Title-Case.md` (`Self-Review.md`, `Working-With-People.md`)
- **Project folders**: `PascalCase` (`MyProject/`)

Dates are always ISO format (`YYYY-MM-DD`) so they sort correctly.

## Frontmatter is mandatory

Every note has YAML frontmatter. Common fields:

```yaml
---
type: conversation | project-session | code-session | insight | reference | meta
date: 2026-04-28
source: claude | chatgpt | gemini | manual | <other>
status: raw | processed | linked
project: "[[Project-Name]]"      # if applicable
tags: [tag1, tag2]
scenarios: [scenario1, scenario2]   # only on insights
related: ["[[other-note]]"]
summary: "One-line description"
---
```

### Status values explained

- `raw` — captured but not yet processed for insights. The insight-extraction skill picks these up.
- `processed` — extraction has run; insights have been pulled out (or none were found).
- `linked` — fully integrated into the graph with bidirectional links to insights.

## Wikilinks > paths

Always use `[[Note Name]]` instead of relative paths. Obsidian resolves them globally and they survive folder reorganizations.

## Scenario tags

Defined in `_meta/Scenarios.md`. Only use tags from that registry on insight notes' `scenarios:` field. If you need a new one, add it there first — don't invent ad-hoc.

## Routing rules (used by the wrap-up skill)

When wrapping up a session, decide where it goes by asking:

1. **Was I in a project folder (one with `CLAUDE.md`)?** → save to that project's `sessions/`.
2. **Else, was the session primarily about coding/debugging?** → `30_Code_Sessions/`.
3. **Else** → `10_Conversations/<source>/`.

Source is `claude` by default; if you imported the conversation from elsewhere, set it accordingly.

## Insight extraction routing

Insights from any source live in `40_Insights/` (flat). What changes is the `scenarios:` field — that's what makes them findable later.

## When in doubt

Default to filing things and add frontmatter; you can always re-file later. The cost of a misfiled note is low; the cost of an unfiled note is that it doesn't exist.
