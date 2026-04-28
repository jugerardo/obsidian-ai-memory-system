---
type: moc
scenario: self-review
description: Insights for performance review prep — accomplishments, growth, evidence of impact
---

# Self-Review

Open this when writing a self-review, prepping for a performance conversation, or doing interview prep.

## Insights tagged `self-review`

```dataview
TABLE
  summary AS "Insight",
  projects AS "Projects",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "self-review")
SORT date DESC
```

## How to use

1. Skim the insights above.
2. Group them by theme (impact, growth, hard problems solved, collaboration).
3. Feed the bundle to Claude with: *"Use these insights to draft my self-review for [period]. Themes I want to emphasize: [...]."*

## Hand-curated highlights

<!-- Pin the strongest examples here for quick access during reviews -->

-

## Notes from past reviews

<!-- What landed well in previous self-reviews; what your manager flagged as growth areas -->

-
