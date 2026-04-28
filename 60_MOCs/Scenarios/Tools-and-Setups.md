---
type: moc
scenario: tools-and-setups
description: Environment, tools, dotfiles, productivity setups
---

# Tools and Setups

## Insights tagged `tools-and-setups`

```dataview
TABLE
  summary AS "Tool / Setup",
  tags AS "Tags",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "tools-and-setups")
SORT date DESC
```
