---
type: project-index
project: "{{Project Name}}"
status: active
started: {{YYYY-MM-DD}}
tags: [project]
---

# {{Project Name}} — Index

This is the project's MOC (Map of Content). Hand-maintained or partly Dataview-generated.

## Status

<!-- One-line current state. E.g., "In design exploration phase. First prototype reviews next week." -->

## Goals

-
-

## Current focus

<!-- 1–3 bullets of what's actively in motion -->

-

## Recent sessions

<!-- Most recent first. The wrap-up skill appends here. -->

-

## Decisions

<!-- Significant decisions, most recent first -->

-

## Linked insights

<!-- Insights from this project that have been promoted to 40_Insights/ -->

```dataview
LIST
FROM "40_Insights"
WHERE contains(file.frontmatter.projects, this.file.link) OR contains(string(file.frontmatter.project), "{{Project Name}}")
SORT file.mtime DESC
```

## Open questions

-

## People involved

-
