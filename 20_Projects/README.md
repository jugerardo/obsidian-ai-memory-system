# 20_Projects

Active, multi-session work toward a defined goal. Each project is a folder.

## Structure of a project folder

```
20_Projects/MyProject/
├── CLAUDE.md            ← Project context Claude reads automatically
├── _index.md            ← MOC: goals, status, links to recent work
├── sessions/            ← Dated session notes (one per working session)
├── decisions/           ← Architecture/design decisions with rationale
└── artifacts/           ← Code snippets, diagrams, deliverables
```

## Starting a new project

1. Create the folder: `20_Projects/<ProjectName>/`
2. Copy `_templates/CLAUDE.md` into it as `CLAUDE.md`
3. Copy `_templates/project-index.md` into it as `_index.md`
4. Fill in goals and current focus
5. Start working — sessions land in `sessions/` via the wrap-up skill

## Archiving

When a project is done or paused indefinitely, move the whole folder to `90_Archive/`. It stays searchable; it's just not in your active view.
