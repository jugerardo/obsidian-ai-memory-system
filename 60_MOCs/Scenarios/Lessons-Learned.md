---
type: moc
scenario: lessons-learned
description: Postmortems and things to do differently next time
---

# Lessons-Learned

Open this at the start of new initiatives, after an incident, or whenever you smell a familiar mistake about to repeat.

## Insights tagged `lessons-learned`

```dataview
TABLE
  summary AS "Lesson",
  projects AS "From",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "lessons-learned")
SORT date DESC
```
