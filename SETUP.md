# Setup

Step-by-step setup for your private vault.

## 1. Clone this template

```bash
# Clone into a working directory
git clone https://github.com/<your-username>/obsidian-ai-memory-system.git my-vault
cd my-vault

# Optional: detach from this remote so your private vault doesn't push to the public template
git remote remove origin
```

If you want your private vault to also be on GitHub (in a private repo), create a new private repo and `git remote add origin <new-url>`.

## 2. Install Obsidian and open the vault

1. Download Obsidian: <https://obsidian.md>
2. In Obsidian: **Open folder as vault** → select your `my-vault` directory.
3. Trust the vault when prompted (allows community plugins to load).

## 3. Install Dataview (recommended)

Dataview lets your scenario MOCs auto-populate from frontmatter — when you tag an insight with `scenarios: [self-review]`, it automatically appears in the Self-Review MOC without manual linking.

1. Settings → **Community plugins** → Turn on community plugins.
2. **Browse** → search **Dataview** → Install → Enable.
3. Settings → **Dataview** → enable **Inline Queries** (optional but useful).

If you skip Dataview, MOCs still work — they're just hand-maintained link lists instead of auto-populating queries.

## 4. Other recommended Obsidian settings

- **Settings → Files & Links → New link format**: `Relative path to file` (keeps links portable when folders move).
- **Settings → Files & Links → Default location for new notes**: `In the current folder`.
- **Settings → Editor → Strict line breaks**: off.

## 5. Fill in `_meta/Me.md`

Open `_meta/Me.md` and replace the placeholder sections with your own info. This file is referenced by every project's CLAUDE.md, so it's the foundation of how Claude works with you.

Fast path: ask Claude to "interview me for my Me.md" and it'll ask 5–8 questions and generate the file.

## 6. Set up the skills

The two skills (`wrap-up` and `insight-extraction`) live as templates in `_templates/`. To make them available as proper Claude Code skills:

### If you use Claude Code

```bash
# Create the skills directory in your home or project
mkdir -p ~/.claude/skills/wrap-up
mkdir -p ~/.claude/skills/insight-extraction

# Copy the skill files, renaming to SKILL.md
cp _templates/skill-wrap-up.md ~/.claude/skills/wrap-up/SKILL.md
cp _templates/skill-insight-extraction.md ~/.claude/skills/insight-extraction/SKILL.md
```

The skills will then be auto-discovered by Claude Code when you trigger them with phrases like "wrap up this session" or "extract insights".

### If you use Cowork

Same idea — drop them into your Cowork skills directory, or paste the content directly into a project's `CLAUDE.md` so Claude reads them as project context.

### If you're using a different AI

The skill files are written as plain markdown procedures. Paste the relevant skill into your AI's system prompt or context when you want to invoke it.

## 7. Start your first project

```bash
mkdir -p 20_Projects/MyFirstProject/{sessions,decisions,artifacts}
cp _templates/CLAUDE.md 20_Projects/MyFirstProject/CLAUDE.md
cp _templates/project-index.md 20_Projects/MyFirstProject/_index.md
```

Open `CLAUDE.md` and `_index.md`, fill in goals and current focus, and you're ready to work.

## 8. Test the wrap-up skill

Have a working session with Claude inside the project folder, then say **"wrap up this session"**. It should:

1. Generate a session summary using the project-session template.
2. Save it to `20_Projects/MyFirstProject/sessions/YYYY-MM-DD_<slug>.md`.
3. Append a link in `_index.md` under "Recent sessions".
4. Reply with the saved file path.

## 9. Schedule insight extraction

Decide on a cadence (weekly is a good default). At your chosen time:

1. Glance at the inbox and triage anything new.
2. Run the insight-extraction skill: "extract insights from new notes".
3. Review the proposed insights — accept, edit, or reject each one.

## Troubleshooting

- **Frontmatter doesn't render in Obsidian**: enable **Settings → Editor → Properties in document → Visible**.
- **Dataview query shows nothing**: check that your insight files have a top-level `scenarios:` field (not nested under another key) and the values match what's in `_meta/Scenarios.md`.
- **Wrap-up keeps saving to the wrong folder**: clarify the routing rules in your project's `CLAUDE.md` or in `_meta/Vault-Conventions.md`.
