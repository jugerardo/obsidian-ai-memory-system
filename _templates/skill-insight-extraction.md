---
name: insight-extraction
description: Scan recent vault notes for atomic, durable insights and create scenario-tagged insight notes. Trigger phrases — "extract insights", "extract insights from new notes", "run insight extraction", "harvest insights".
---

# Insight-extraction skill

When the user says **"extract insights"**, **"extract insights from new notes"**, **"run insight extraction"**, or **"harvest insights"**, run this procedure to scan recent notes, identify atomic insights, and create them as standalone files in `40_Insights/`.

This skill is intentionally **deliberate, not automatic**. It produces drafts; the user accepts/edits/rejects each one. Quality is the goal — bloated insight folders are the failure mode.

## Vault access

All reads and writes to the Obsidian vault must go through the **obsidian-mcp**. Do not use file system tools (Read, Write, Edit, Bash) to touch vault files.

Key operations and their obsidian-mcp equivalents:

| What you need to do | obsidian-mcp tool to use |
|---|---|
| Read an existing note | `read_note` (or equivalent get/fetch tool) |
| Create a new note | `create_note` (or equivalent write tool) |
| Update an existing note (frontmatter or body) | `update_note` / `patch_note` (or equivalent) |
| List notes in a folder | `list_notes` / `list_files` |
| Search across notes | `search_notes` / `search` |

If the obsidian-mcp is not connected or returns an error, **stop and tell the user** — do not fall back to file system tools silently.

## Inputs

- The vault root.
- A scope filter (default: all notes in `10_Conversations/`, `20_Projects/**/sessions/`, `30_Code_Sessions/` with `status: raw`).
- The Scenarios registry at `_meta/Scenarios.md` — the only valid values for `scenarios:` field.
- Existing insights in `40_Insights/` (for deduplication).

## Step 1: Find candidate notes

Use the obsidian-mcp `search_notes` or `list_notes` tool to find all notes matching the scope filter where the frontmatter has `status: raw`. Sort by date.

If the user gave a narrower scope ("extract from this project's sessions" or "extract from this week's notes"), respect it.

If no notes are found with `status: raw`, tell the user "Nothing new to extract" and stop.

## Step 2: Read the Scenarios registry and build the deduplication index

### 2a. Load the Scenarios registry

Use obsidian-mcp `read_note` to open `_meta/Scenarios.md` and load the **active scenarios list** (their names, purposes, and inclusion criteria). You may **only** use scenario names from this list when tagging insights.

### 2b. Build a deduplication index

Use obsidian-mcp `list_notes` to get all files in `40_Insights/`, then use `read_note` to read the frontmatter of each one (you only need `summary:`, `scenarios:`, and `tags:` — not the full body). Build an in-memory index:

```
existing_insights = [
  { path, summary, scenarios, tags },
  ...
]
```

Doing this once up front is more efficient than running a live search per candidate in step 3c, and it also lets you catch cross-candidate duplicates (two candidates in the same batch that express the same lesson).

## Step 3: For each candidate note

For each note in scope:

### 3a. Read the note's `## Candidate insights` section

Use obsidian-mcp `read_note` to open the note. The `## Candidate insights` section is the load-bearing input. The wrap-up skill flagged these inline. They're starting points, not final insights.

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

Use the deduplication index from step 2b — no additional vault reads needed here.

**Against existing insights:**

- **Near-duplicate (≥70% overlap with an existing summary)**: mark as `UPDATE existing [[slug]]` rather than `NEW`. Don't create a new file — augment the existing one in step 5.
- **Partial overlap (40–70%)**: keep as `NEW` but set `related: ["[[existing-slug]]"]` in the draft frontmatter to surface the connection.
- **No meaningful overlap (<40%)**: proceed as `NEW`.

**Against other candidates in the current batch:**

After drafting each candidate, also compare it against candidates already drafted in this run. If two candidates from different source notes express the same core lesson, merge them into a single draft, listing both source notes in `source:`. Don't create two near-identical insight files.

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

### 3f. Scan for emerging scenario patterns

After processing all candidates from all source notes, look across the full draft batch for scenario gaps — don't wait for a single misfit to surface them.

1. Group all drafted candidates by the scenarios you tentatively assigned.
2. Look for clusters of **2 or more** candidates that share a topic, tag, or theme but are awkwardly assigned to a broad catch-all scenario (like `lessons-learned` or `decisions`) only because no more specific scenario exists.
3. Also gather any candidates you flagged as `[no-scenario]` during step 3b.
4. For each cluster or group of misfits, ask: *would a new scenario serve these better, and would the user plausibly query for it separately?* If yes, draft a scenario proposal (name, purpose, inclusion criteria).

Hold these proposals — you'll present them in step 4 alongside the insight drafts. This replaces the per-candidate interruption that step 6 used to trigger.

If only one candidate needs a new scenario and no cluster supports it, skip the proposal. One insight is not enough to justify a new scenario bucket.

## Step 4: Present drafts to the user

Do **not** save insight files yet. Present the drafts for review.

If step 3f produced scenario proposals, lead with them before the insight list:

```
Scenario proposal: <name>
Purpose: <when the user would query for it>
Inclusion criteria: <what belongs here>
Affects N candidates: <titles>
[Action: Approve / Reject]
```

Then present each insight draft:

```
Found N candidate insights from M source notes.

Insight 1 / N: <title>
Source: <source note path>
Scenarios: [scenario1, scenario2]
Summary: <one-sentence>
Status: NEW | UPDATE existing [[insight-name]] | MERGE with [[other-candidate]]

The insight: <body>
[Action: Accept / Edit / Reject]
```

Wait for the user's decision on scenario proposals first, then insights. Apply any approved scenario names before assigning them to insights. Apply edits as instructed before saving.

## Step 5: Save accepted insights and update sources

For each accepted insight:

1. Use obsidian-mcp `create_note` to write the insight file to `40_Insights/<slug>.md`.
2. Use obsidian-mcp `update_note` to update the **source note's frontmatter**:
   - Flip `status: raw` → `status: processed`.
   - Add `related:` link to the new insight: `related: ["[[40_Insights/<slug>]]"]`.
3. If the source note has `## Candidate insights`, use obsidian-mcp `update_note` to mark each bullet that became an insight: `- [x] <description> → [[<slug>]]`.

For accepted updates to existing insights:

1. Use obsidian-mcp `update_note` to append to the existing insight's body or `Related` list.
2. Add the new source to the existing insight's `source:` field (now a list) in the same update.
3. Update the source note as above.

For rejected candidates:

- Don't create the file. Use obsidian-mcp `update_note` to mark the bullet in the source note: `- [~] <description> (rejected: <reason>)`.

## Step 6: Save approved scenarios

For each scenario proposal the user approved in step 4:

1. Use obsidian-mcp `update_note` to append it to `_meta/Scenarios.md` under `## Active scenarios`.
2. Apply the new scenario name to the relevant insight drafts before saving them in step 5.

Scenario proposals come from the batch-level pattern scan in step 3f — not from per-candidate interruptions mid-processing. If a single candidate has no scenario fit and no batch pattern supported a proposal, assign the closest existing scenario and add a note in the insight's body about the imperfect fit. Don't block on it.

## Step 7: Mark fully-processed notes as linked

Use obsidian-mcp `update_note` to flip source notes where **all** candidate insights have been processed (accepted, rejected, or merged) to `status: linked`. Notes with un-processed candidates stay at `processed`.

## Step 8: Reply

Reply with:

1. Number of insights created (new + updates + merges).
2. Number of candidates deduplicated (merged into existing insights or cross-batch merges).
3. Number of candidates rejected.
4. Number of source notes updated.
5. Any new scenarios added to the registry.
6. A list of saved insight paths.

## What this skill does NOT do

- It does **not** modify the body of existing source notes (only frontmatter and the `## Candidate insights` checkboxes).
- It does **not** create new MOCs or tags outside the registry.
- It does **not** silently filter — every rejection is logged in the source note.

## Cadence

Recommended: weekly. Run after a Sunday/Monday triage of the inbox, when you have 20+ minutes to focus.

Don't run mid-session. Insight extraction needs cross-vault context (deduplication) and deliberate review — both suffer when rushed.
