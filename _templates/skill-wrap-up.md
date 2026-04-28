---
name: wrap-up
description: Save the current session as a structured note in the Obsidian vault, routed to the right folder based on context. Trigger phrases — "wrap up", "wrap up this session", "save this session", "save our work".
---

# Wrap-up skill

When the user says **"wrap up"**, **"wrap up this session"**, **"save this session"**, or any close variant, run this procedure to capture the session as a structured note in their vault.

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

- The full conversation context (what we worked on this session).
- The vault root (typically `AI Memory/` or wherever the user's vault lives).
- The current working directory — used to detect whether we're inside a project folder.
- Today's date in `YYYY-MM-DD` (use the system date — do not guess).

## Step 1: Decide where to save

Routing rules, in order:

1. **If the working directory is inside a project folder** (a folder under `20_Projects/` that contains a `CLAUDE.md`):
   - Type: `project-session`
   - Save to: `20_Projects/<ProjectName>/sessions/YYYY-MM-DD_<slug>.md`
   - Use template: `_templates/project-session.md`

2. **Else, if the session was primarily about coding/debugging** (writing code, debugging errors, exploring a library, refactoring):
   - Type: `code-session`
   - Save to: `30_Code_Sessions/YYYY-MM-DD_<slug>.md`
   - Use template: `_templates/code-session.md`

3. **Else** (discussion, brainstorm, exploration without code):
   - Type: `conversation`
   - Save to: `10_Conversations/<source>/YYYY-MM-DD_<slug>.md` (where `<source>` is `claude`, `chatgpt`, `gemini`, or `other`)
   - Use template: `_templates/conversation.md`

If routing is ambiguous (e.g., a discussion that involved some code), **ask the user once** which folder they want it in. Do not guess silently.

## Step 2: Generate the slug

The slug is a kebab-case fragment, ≤ 6 words, that captures the session's topic. Examples:

- `obsidian-vault-design`
- `fix-react-hydration-error`
- `payments-redesign-stakeholder-review`

Avoid generic slugs like `discussion` or `chat`. If the topic is fuzzy, ask the user for a slug.

## Step 3: Fill the template

Open the chosen template. Fill in:

### Frontmatter

- `date`: today's date (`YYYY-MM-DD`).
- `type`: matches the routing decision.
- `source`: `claude` unless context says otherwise.
- `status`: always `raw` (insight extraction will flip it later).
- `project`: `"[[ProjectName]]"` if applicable, else omit or empty.
- `tags`: a few relevant tags. Be conservative — don't pad.
- `summary`: one sentence. The user should be able to read this and remember the session.

### Body sections

Faithfully summarize the session. Specifically:

- **Goal / Context**: what the user set out to do this session.
- **What happened / What we discussed**: substantive recap, not a transcript. Include code snippets, decisions, and dead ends in their respective sections.
- **Decisions made** (project-session): significant calls.
- **Outcomes**: completed / in progress / blocked.
- **Candidate insights**: this is the load-bearing section for the insight-extraction skill. List anything atomic and durable that might deserve its own insight note. Format: one bullet per candidate, each one a one-line description that could become an insight title. Be generous here — the extraction skill will filter.
- **Next steps / Follow-ups**: what's queued up.

### Code-session specifics

- Inline only the most useful code/commands. Don't paste entire files.
- Capture errors and their root causes — these are gold for `code-troubleshooting` insights.

## Step 4: Save the file

Use the obsidian-mcp `create_note` tool to write the file to the path from Step 1. Use ISO date prefix and the slug, e.g., `2026-04-28_obsidian-vault-design.md`.

## Step 5: Update affected indexes

If routed to a project folder:

- Use obsidian-mcp `read_note` to open the project's `_index.md`.
- Use obsidian-mcp `update_note` to append a link to the new session under `## Recent sessions`, format: `- [[YYYY-MM-DD_slug|YYYY-MM-DD: short title]]`.
- If the session changed the project's direction, revise the `## Current focus` section in the same update call.
- If a significant decision was made and saved separately in `decisions/`, also append it under `## Decisions`.

## Step 6: Reply

Reply to the user with:

1. **The saved file path** (so they can verify and open it).
2. **A 1-sentence summary** of what was captured.
3. **The number of candidate insights flagged** for the next extraction pass.

Do NOT recap the entire session in your reply. The note exists for that.

## What this skill does NOT do

- It does **not** create insight notes. Candidates are flagged inline; the insight-extraction skill harvests them later.
- It does **not** modify `40_Insights/` or any other file outside the session's home folder and the project's `_index.md`.
- It does **not** delete or restructure existing notes.

## Edge cases

- **Multiple topics in one session**: ask whether to split into multiple notes or capture as one with multiple subsections.
- **Session was very short / nothing substantive**: ask whether the user actually wants this saved. A 4-message exchange usually doesn't warrant its own note.
- **Vault root unclear**: ask the user where their vault lives.
- **No `CLAUDE.md` in working dir but the user names a project**: trust them — save to that project's `sessions/`.
