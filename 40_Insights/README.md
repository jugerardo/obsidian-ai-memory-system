# 40_Insights

The knowledge layer. Atomic notes — one idea per file, written in your own words, addressable from anywhere in the vault.

## What belongs here

- Durable lessons, not session logs
- Distilled findings that will outlive the project they came from
- Things you want to be able to pull out by *scenario* (self-review, code-tips, working-with-Sarah, etc.)

## What does NOT belong here

- Raw conversation transcripts → `10_Conversations/`
- Session-specific notes → `20_Projects/.../sessions/` or `30_Code_Sessions/`
- External reference material → `50_References/`

## Why flat instead of nested folders

A single insight often serves multiple scenarios. "Sarah prefers async written updates" is simultaneously how-to-work-with-Sarah, stakeholder-feedback, *and* self-review evidence. Filing it once in `people/` would lose it for the other two queries.

So insights live flat. **Scenarios** (controlled vocabulary in `_meta/Scenarios.md`) are tags in frontmatter, and `60_MOCs/Scenarios/` provides curated views per use case.

## Naming

`kebab-case-slug.md` — name it like a sentence fragment: `never-mock-network-in-integration-tests.md`, `sarah-prefers-async-updates.md`.

## Frontmatter

See `_templates/insight.md` for the full schema. The load-bearing field is `scenarios:` — that's what powers cross-cutting queries.

## How insights get created

Two paths:

1. **Manually** — you write one when you've learned something durable.
2. **Insight-extraction skill** — runs over notes flagged `status: raw`, identifies atomic insights, creates files here, links back to source.

See `_templates/mem-insight-extraction.md`.
