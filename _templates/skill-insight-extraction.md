---
name: insight-extraction
description: Scan recent vault notes for atomic, durable insights and create scenario-tagged insight notes. Trigger phrases — "extract insights", "extract insights from new notes", "run insight extraction", "harvest insights".
---

# Insight-extraction skill

When the user says **"extract insights"**, **"extract insights from new notes"**, **"run insight extraction"**, or **"harvest insights"**, run this procedure to scan recent notes, identify atomic insights, and create them as standalone files in `40_Insights/`.

This skill is intentionally **deliberate, not automatic**. It produces drafts; the user accepts/edits/rejects each one. Quality is the goal — bloated insight folders are the failure mode.

## Inputs

- The vault root.
- A scope filter (default: all notes in `10_Conversations/`, `20_Projects/**/sessions/`, `30_Code_Sessions/` with `status: raw`).
- The Scenarios registry at `_meta/Scenarios.md` — the only valid values for `scenarios:` field.
- Existing insights in `40_Insights/` (for deduplication).

## Step 1: Find candidate notes

Read all notes matching the scope filter where the frontmatter has `status: raw`. Sort by date.

If the user gave a narrower scope ("extract from this project's sessions" or "extract from this week's notes"), respect it.

If no notes are found with `status: raw`, tell the user "Nothing new to extract" and stop.

## Step 2: Read the Scenarios registry

Open `_meta/Scenarios.md` and load the **active scenarios list** (their names, purposes, and inclusion criteria). You may **only** use scenario names from this list when tagging insights.

If you find a candidate insight that genuinely doesn't fit any active scenario, follow the "propose new scenario" procedure in Step 6.

## Step 3: For each candidate note

For each note in scope:

### 3a. Read the note's `## Candidate insights` section

This is the load-bearing input. The wrap-up skill flagged these inline. They're starting points, not final insights.

If the note has no `## Candidate insights` section but you spot atomic insights in the body, you may propose them — but be conservative.

### 3b. For each candidate

Decide: **does this deserve its own insight note?**

A candidate qualifies if all of these are true:

- **Atomic** — it's one idea, not a cluster. ("Sarah prefers async updates" qualifies. "How to work effectively with the design team" does not — that's a cluster.)
- **Durable** — it'll still be useful in 6+ months. ("This sprint's deadline is Thursday" does not qualify.)
- **Reusable** — it applies beyond the immediate context. (A bug fix specific to one PR does not. A debugging technique that generalized does.)
- **Non-obvious** — it's not something Claude/anyone could regenerate from general knowledge. ("Use semicolons in JavaScript" does not qualify. "Our auth middleware silently swallows errors when the DB is unreachable — check the connection pool first" does.)

If the candidate fails any of these, skip it.

### 3c. Check for duplicates

Before creating a new insight, search `40_Insights/` for existing notes with similar titles or summaries. If a near-duplicate exists, prefer to **augment the existing insight** (add a related source, expand the body) over creating a new file.

Use a simple heuristic: if the existing insight's `summary:` and the candidate's stated lesson would overlap by ≥70%, treat as duplicate.

### 3d. Pick scenarios

For each insight, pick **all applicable scenarios** from the registry. Most insights belong to 1–3 scenarios. Going over 4 is a signal that the insight is too broad — consider splitting.

For each scenario you assign, you should be able to answer: *"In what situation will the user query for this scenario, and would they want this insight back?"* If you can't, don't assign it.

### 3e. Draft the insight file

Use `_templates/insight.md`. Fill:

- **Filename**: `kebab-case-sentence-fragment.md`. Be specific and short — the title should be readable on its own. Examples:
  - `sarah-prefers-async-written-updates.md`
  - `auth-middleware-swallows-db-errors.md`
  - `react-strictmode-double-renders-effects-in-dev.md`

- **Frontmatter**:
  - `type: insight`
  - `date`: source note's date (or today, if compiled from multiple sources)
  - `status: linked` (insights are created already-linked)
  - `source: "[[<source-note>]]"` — the note this came from. If multiple sources contributed, list all.
  - `scenarios:` — from the registry, as decided in 3d.
  - `projects:` — if project-specific, link them. Empty if cross-project.
  - `people:` — `["[[Name]]"]` if about a specific person.
  - `tags:` — free-form (tech, topic). Don't reuse scenario names here.
  - `related:` — links to other insights you noticed are connected. Optional.
  - `summary:` — one-sentence statement of the insight.

- **Body**: fill the template sections. Keep it tight — these are atomic notes, not essays.
  - **The insight**: one paragraph.
  - **Why it's true / where I learned it**: brief. Link to source.
  - **How to apply**: when it kicks in.
  - **Caveats**: when it doesn't apply. Insights without caveats are usually overgeneralized — try to think of one.

## Step 4: Present drafts to the user

Do **not** save insight files yet. Present the drafts for review:

```
Found N candidate insights from M source notes.

Insight 1 / N: <title>
Source: <source note path>
Scenarios: [scenario1, scenario2]
Summary: <one-sentence>
Status: NEW | UPDATE existing [[insight-name]]

The insight: <body>
[Action: Accept / Edit / Reject]
```

Wait for the user's decision per insight. Apply edits as instructed before saving.

## Step 5: Save accepted insights and update sources

For each accepted insight:

1. Write the insight file to `40_Insights/<slug>.md`.
2. Update the **source note's frontmatter**:
   - Flip `status: raw` → `status: processed`.
   - Add `related:` link to the new insight: `related: ["[[40_Insights/<slug>]]"]`.
3. If the source note has `## Candidate insights`, leave the bullets in place but mark each that became an insight: `- [x] <description> → [[<slug>]]`.

For accepted updates to existing insights:

1. Append to the existing insight's body or `Related` list.
2. Add the new source to the existing insight's `source:` field (now a list).
3. Update the source note as above.

For rejected candidates:

- Don't create the file. In the source note, mark the bullet: `- [~] <description> (rejected: <reason>)`.

## Step 6: Propose new scenarios (if needed)

If during processing you encounter a candidate insight that genuinely doesn't fit any active scenario, **before** creating it:

1. Propose a new scenario with: `name` (kebab-case), `purpose` (when the user would query for it), `inclusion criteria`.
2. Ask the user to approve.
3. If approved, append the new scenario to `_meta/Scenarios.md` under `## Active scenarios`.
4. Use it for the candidate insight.

Be stingy with new scenarios. Adding a new one should require ≥2 candidate insights that need it. If only one insight needs a new scenario, consider whether it really needs its own bucket, or if an existing one stretches to fit.

## Step 7: Mark fully-processed notes as linked

Source notes where **all** candidate insights have been processed (accepted, rejected, or merged) get their `status:` flipped to `linked`. Notes with un-processed candidates stay at `processed`.

## Step 8: Reply

Reply with:

1. Number of insights created (new + updates).
2. Number of candidates rejected.
3. Number of source notes updated.
4. Any new scenarios added to the registry.
5. A list of saved insight paths.

## What this skill does NOT do

- It does **not** modify the body of existing source notes (only frontmatter and the `## Candidate insights` checkboxes).
- It does **not** create new MOCs or tags outside the registry.
- It does **not** silently filter — every rejection is logged in the source note.

## Cadence

Recommended: weekly. Run after a Sunday/Monday triage of the inbox, when you have 20+ minutes to focus.

Don't run mid-session. Insight extraction needs cross-vault context (deduplication) and deliberate review — both suffer when rushed.
