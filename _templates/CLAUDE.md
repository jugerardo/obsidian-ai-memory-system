# {{Project Name}}

Drop this file into a new project folder as `CLAUDE.md`. Claude will read it automatically when working in this directory.

## Project identity

- **Project name**: `{{Project Name}}` — must match the folder name under `20_Projects/` exactly. Used by the import and wrap-up skills to route notes to this project. Verify with obsidian-mcp `list_notes` on `20_Projects/`.
- **Vault link**: `[[{{Project Name}}]]` — use this in frontmatter `project:` fields and wikilinks throughout the vault.

> **If the project name above still says `{{Project Name}}`**, ask the user for it before doing anything else: "What's the name of this project in your vault? It should match the folder name under `20_Projects/`." Then use obsidian-mcp `list_notes` on `20_Projects/` to verify the folder exists. Fill the name in once confirmed and use it for the rest of the session.

## Read this first

Before doing anything, read these files in order using obsidian-mcp `read_note` — do not use file system tools:

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

<!-- Anything that overrides or extends the general vault conventions for this project. Leave blank if nothing is project-specific. -->

- **Stack**: <!-- e.g., React + TypeScript + Tailwind — helps Claude give relevant suggestions without asking -->
- **Code style**: <!-- e.g., Follow patterns in src/components; ask before introducing new libraries -->
- **What lives outside this vault**: <!-- e.g., Source code is in github.com/org/repo — don't try to create code files here -->
- **Out of scope**: <!-- e.g., Backend changes — ask before suggesting -->

## Where to save work in this project

- **Session summaries** → `sessions/YYYY-MM-DD_<slug>.md`. Use obsidian-mcp `create_note` with `_templates/project-session.md` as the template.
- **Decisions** → `decisions/YYYY-MM-DD_<slug>.md`. Use obsidian-mcp `create_note`. One decision per file. Link back to the session note that produced it.
- **Written artifacts** → `artifacts/`. Use obsidian-mcp `create_note`. Only vault-native summaries or distillations belong here — e.g. a brief capturing the key decisions from a spec, or a prompt worth keeping. If a markdown file already exists as part of the project's actual output (a README, a spec in the repo, a design doc in Figma), don't copy it here. Instead, reference it from the session note with a link or path. The test: would this note be useful to search or link to in Obsidian independently of the project? If not, it doesn't belong in `artifacts/`.
- **Code, binaries, exports, images** → these do **not** belong in Obsidian. Keep them in the project's own repo or storage. Reference them from session notes with a file path or external link — don't copy them here.
- **Candidate insights** → flag inline in the session note under `## Candidate insights`. Don't create insight files directly — that's the insight-extraction skill's job.

## Wrap-up command

When I say **"wrap up"**, **"wrap up this session"**, or **"save this session"**:

1. Use obsidian-mcp `create_note` to save a session summary to `sessions/YYYY-MM-DD_<slug>.md` using `_templates/project-session.md` as the template. Full frontmatter required: `type: project-session`, `project: "[[{{Project Name}}]]"`, `status: raw`.
2. Use obsidian-mcp `read_note` + `update_note` to update `_index.md`:
   - Append the new session under **Recent sessions**: `- [[YYYY-MM-DD_slug|YYYY-MM-DD: short title]]`.
   - Revise **Current focus** if the session changed direction.
3. Review the **Key decisions so far** section in this file. For any inline one-liners that involve real reasoning or tradeoffs, use obsidian-mcp `create_note` to save them to `decisions/YYYY-MM-DD_<slug>.md`, then use obsidian-mcp `update_note` to replace the inline text in this file with a wikilink: `[[YYYY-MM-DD_slug|Short description]]`.
4. Reply with the saved file path so I can verify.

Do not use file system tools (Read, Write, Edit, Bash) for any of the above — obsidian-mcp only.

Full skill reference: `_templates/mem-wrap-up.md`.

## Active people / stakeholders

<!-- Names, roles, and how to work with them. If you have insight notes about them in 40_Insights/, use obsidian-mcp `read_note` to look them up and link here — don't duplicate the content. -->

-

## Key decisions so far

<!-- Two formats allowed:
  - Minor calls (no significant tradeoff): one-liner inline. e.g. "- Chose Tailwind over plain CSS for speed."
  - Significant calls (has reasoning or tradeoffs): save to decisions/ and link here. e.g. "- [[2026-04-28_auth-approach|Auth approach]]"
  The wrap-up skill will move any inline one-liners that grew into real decisions to decisions/ automatically. -->

-

## Open questions

<!-- Things we don't yet know that are blocking progress or need a decision. Remove items once resolved — don't let this become a graveyard. -->

-
