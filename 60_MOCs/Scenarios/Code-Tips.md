---
type: moc
scenario: code-tips
description: Durable coding patterns useful across projects
---

# Code-Tips

Open this when starting a new coding project, onboarding into unfamiliar tech, or just looking for "how have I done this well before?"

## Insights tagged `code-tips`

```dataview
TABLE
  summary AS "Tip",
  tags AS "Stack",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "code-tips")
SORT date DESC
```

## Filter by stack

To pull only React tips:

```dataview
LIST summary
FROM "40_Insights"
WHERE contains(scenarios, "code-tips") AND contains(tags, "react")
```

(Edit the `"react"` to filter by other tags.)
