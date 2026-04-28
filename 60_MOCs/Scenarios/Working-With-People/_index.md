---
type: moc
scenario: working-with-people
description: Per-person collaboration notes — communication style, preferences, history
---

# Working with People

Open this before a 1:1, when joining a new team, or when navigating a tricky collaboration.

## Per-person pages

<!-- Create one file per person you work with regularly. E.g., Sarah.md, Alex.md.
     Each person's page is itself an MOC pulling all their tagged insights. -->

-

## All `working-with-people` insights

```dataview
TABLE
  summary AS "Insight",
  people AS "Person",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "working-with-people")
SORT people ASC, date DESC
```

## How to add a person

1. Create `60_MOCs/Scenarios/Working-With-People/<Name>.md`.
2. Use this template:

```markdown
---
type: moc
scenario: working-with-people
person: "[[<Name>]]"
---

# <Name>

## Role / context

## Communication preferences

## What motivates them

## Watch-outs

## Active topics

## All insights about <Name>

\`\`\`dataview
LIST summary
FROM "40_Insights"
WHERE contains(people, "[[<Name>]]")
SORT date DESC
\`\`\`
```
