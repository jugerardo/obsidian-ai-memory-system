---
type: meta
description: Controlled vocabulary for the `scenarios:` field on insight notes
---

# Scenarios

This is the **only** valid set of values for the `scenarios:` field on insight notes. The insight-extraction skill must pick from this list. If a new scenario is needed, it gets proposed and added here first — do not invent ad-hoc tags.

## How to use this list

- An insight can have multiple scenarios (most do).
- Each scenario has a **purpose** (when you'd query for it) and **inclusion criteria** (what kinds of insights belong).
- When extracting, ask: *"In what future situation will I want this insight back?"* That's the scenario.

## Active scenarios

### `self-review`

**Purpose**: Performance review prep. Pull the bundle when writing a self-review or interview prep.

**Include**: Accomplishments, projects shipped, growth moments, recognized impact, hard problems solved, evidence of skill development.

---

### `code-tips`

**Purpose**: Onboarding into a new project, applying durable coding patterns across work.

**Include**: Reusable techniques, idioms, performance patterns, architectural lessons, language- or framework-specific gotchas you'll hit again.

**Tag with `tags:`** the relevant tech (e.g., `[react, typescript]`) so you can filter further.

---

### `code-troubleshooting`

**Purpose**: Re-diagnosis. When you hit a similar error or smell, pull this bundle.

**Include**: Bugs with non-obvious root causes, debugging approaches that worked, environmental quirks, dependency conflicts, "I lost three hours to this and I don't want to repeat it."

---

### `design-portfolio`

**Purpose**: Building or updating a design portfolio / case studies.

**Include**: Design decisions with rationale, before/after impact, user research findings worth showcasing, design system contributions, cross-functional collaboration moments.

---

### `stakeholder-feedback`

**Purpose**: Understanding patterns in how stakeholders react to your work; informing future communication.

**Include**: Direct feedback received from PMs, leadership, customers, peers. Both positive (what landed) and constructive (what to refine).

---

### `working-with-people`

**Purpose**: Adapting your approach to specific collaborators. Pulled before a 1:1 or when joining a new team.

**Include**: Communication preferences per person, what motivates them, what irritates them, working hours, decision-making style. Tag with the person via `people: ["[[Name]]"]`.

---

### `decisions`

**Purpose**: Future "why did we decide X?" questions. Auditable history of significant calls.

**Include**: Architecture/design decisions with rationale, alternatives considered, tradeoffs accepted. Anything that would belong in an ADR (Architecture Decision Record).

---

### `lessons-learned`

**Purpose**: Postmortems. Avoiding repeat mistakes.

**Include**: Things that went wrong and what you'd do differently. Process failures, missed signals, assumption-violations.

---

### `processes`

**Purpose**: Operational know-how that's not "code" but is durable.

**Include**: How to run a particular kind of meeting, deployment runbooks, "the way we do X here," recurring workflows.

---

### `tools-and-setups`

**Purpose**: Reproducing or improving your environment.

**Include**: Editor configs that worked, plugin combos, dotfile snippets, productivity tricks, tool comparisons with the verdict.

---

## How to propose a new scenario

When the insight-extraction skill encounters an insight that genuinely doesn't fit any existing scenario, it should:

1. Propose a new scenario with: name (kebab-case), purpose, inclusion criteria.
2. Append it to this file under `## Active scenarios`.
3. Use it for the new insight.

Review proposed scenarios occasionally — collapse near-duplicates, retire ones that haven't been used.

## Retired scenarios

(None yet.)
