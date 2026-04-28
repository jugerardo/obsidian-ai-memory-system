# {{Project Name}}

Drop this file into a new project folder as `CLAUDE.md`. Claude will read it automatically when working in this directory.

## Read this first

Before doing anything, read these files in order:

1. `[[../../_meta/Me.md]]` — who I am and how I prefer to work
2. `[[../../_meta/Vault-Conventions.md]]` — folder roles, naming, frontmatter rules
3. `[[_index.md]]` — this project's goals, status, and recent work

## Project goals

<!-- What does done look like? -->

-
-

## Current focus

<!-- What we're actively working on right now. Update this as it changes. -->

-

## Project-specific conventions

<!-- Anything that overrides or extends the general vault conventions for this project -->

- **Stack**: <!-- e.g., React + TypeScript + Tailwind -->
- **Code style**: <!-- e.g., Use existing patterns in src/components; ask before introducing new libraries -->
- **Out of scope**: <!-- e.g., Backend changes — ask before suggesting -->

## Where to save work in this project

- **Session summaries** → `sessions/YYYY-MM-DD_<slug>.md` (use `_templates/project-session.md`)
- **Decisions** → `decisions/YYYY-MM-DD_<slug>.md`
- **Code/artifacts** → `artifacts/`
- **Candidate insights** → flag inline in the session note under `## Candidate insights`. Don't create insight files yet — that's the insight-extraction skill's job.

## Wrap-up command

When I say **"wrap up"**, **"wrap up this session"**, or **"save this session"**, follow the wrap-up procedure:

1. Generate a session summary using `_templates/project-session.md` as the template.
2. Save to `sessions/YYYY-MM-DD_<slug>.md` with full frontmatter (`type: project-session`, `project: "[[{{Project Name}}]]"`, `status: raw`).
3. Update `_index.md`:
   - Append the new session to the **Recent sessions** list.
   - Revise **Current focus** if the session changed direction.
4. Reply with the saved file path so I can verify.

Full skill reference: `_templates/skill-wrap-up.md`.

## Active people / stakeholders

<!-- Names + role + how to work with them. Link to `40_Insights/` notes about them when applicable. -->

-

## Key decisions so far

<!-- Hand-curated link list. The decisions/ folder has the originals. -->

-

## Open questions

<!-- Things we don't yet know that block progress -->

-
