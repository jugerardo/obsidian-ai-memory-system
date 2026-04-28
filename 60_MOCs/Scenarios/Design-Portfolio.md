---
type: moc
scenario: design-portfolio
description: Insights and work suitable for portfolio / case studies
---

# Design-Portfolio

Open this when building or refreshing your design portfolio.

## Insights tagged `design-portfolio`

```dataview
TABLE
  summary AS "Insight",
  projects AS "Projects",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "design-portfolio")
SORT date DESC
```

## Project case study candidates

<!-- Hand-curated list of projects that are portfolio-worthy. Link to their _index.md. -->

-

## Hand-curated portfolio narratives

<!-- Short bullets summarizing how a project would be told as a case study: problem → approach → impact -->

-
