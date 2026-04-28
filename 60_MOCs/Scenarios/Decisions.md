---
type: moc
scenario: decisions
description: Significant decisions with rationale — for future "why did we...?" questions
---

# Decisions

Open this when you need to recall *why* you went one way instead of another.

## Insights tagged `decisions`

```dataview
TABLE
  summary AS "Decision",
  projects AS "Project",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "decisions")
SORT date DESC
```

## Per-project decisions

Project-specific decision logs live in each project's `decisions/` folder. The insights here are the cross-cutting / durable lessons.
