---
type: moc
scenario: code-troubleshooting
description: Bugs, issues, and non-obvious fixes worth keeping
---

# Code-Troubleshooting

Open this when you've hit something weird and want to check whether you've solved it before.

## Insights tagged `code-troubleshooting`

```dataview
TABLE
  summary AS "Issue / Fix",
  tags AS "Stack",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "code-troubleshooting")
SORT date DESC
```

## How to use

When debugging:

1. Search this list for keywords matching the error/symptom.
2. If a match exists, that note has the fix.
3. If not, when you eventually fix it, run insight extraction to add it.
