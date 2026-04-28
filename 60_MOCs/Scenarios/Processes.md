---
type: moc
scenario: processes
description: Operational know-how — how to run things, runbooks, recurring workflows
---

# Processes

## Insights tagged `processes`

```dataview
TABLE
  summary AS "Process / Workflow",
  tags AS "Tags",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "processes")
SORT date DESC
```
