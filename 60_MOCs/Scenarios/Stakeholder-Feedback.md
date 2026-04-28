---
type: moc
scenario: stakeholder-feedback
description: Feedback received from PMs, leadership, customers, peers
---

# Stakeholder-Feedback

Open this before stakeholder reviews, when prepping for a tough conversation, or when looking for patterns in how your work lands.

## Insights tagged `stakeholder-feedback`

```dataview
TABLE
  summary AS "Feedback",
  people AS "From",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "stakeholder-feedback")
SORT date DESC
```

## Filter by person

```dataview
LIST summary
FROM "40_Insights"
WHERE contains(scenarios, "stakeholder-feedback") AND contains(people, "[[Name]]")
```

(Replace `[[Name]]` with the person you're prepping for.)
