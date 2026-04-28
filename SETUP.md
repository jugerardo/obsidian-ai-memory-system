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

## 3. Install and configure Dataview

Dataview is the plugin that makes scenario MOCs auto-populate. When you tag an insight with `scenarios: [self-review]`, it automatically shows up in the Self-Review MOC — no manual linking. Without it, every MOC update would be hand-maintained.

### What Dataview does

It treats your vault like a database. Frontmatter fields become queryable columns. The MOC files in `60_MOCs/Scenarios/` already contain example queries — Dataview reads them and renders the results inline.

### Install it

1. Open Obsidian.
2. **Settings** (gear icon, bottom-left) → **Community plugins**.
3. If you see "Restricted mode is on", click **Turn on community plugins** and confirm.
4. Click **Browse**.
5. Search **Dataview**.
6. Click the result → **Install** → **Enable**.

### Configure it

1. **Settings** → **Dataview** (now appears in the left sidebar under "Community plugins").
2. Recommended settings:
   - **Enable JavaScript Queries**: off (you don't need it; codeblock queries are enough).
   - **Enable Inline Queries**: on (lets you embed `= this.file.name` style queries in any note).
   - **Enable Inline Field Highlighting**: on.
   - **Render Null As**: leave blank or set to `—`.
3. Close settings.

### Verify it's working

Open `60_MOCs/Scenarios/Self-Review.md`. You should see a code block like this:

````markdown
```dataview
TABLE
  summary AS "Insight",
  projects AS "Projects",
  date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "self-review")
SORT date DESC
```
````

In **Reading view** (Ctrl/Cmd+E to toggle), this should render as an empty table (you don't have any insights yet — that's expected). If you see the raw `dataview` codeblock instead of a rendered table, Dataview isn't enabled. Re-check the plugin settings.

### Test it with a fake insight

To verify queries actually work:

1. Create a file at `40_Insights/test-insight.md`:

   ```markdown
   ---
   type: insight
   date: 2026-04-28
   status: linked
   scenarios: [self-review, code-tips]
   summary: "Test insight to verify Dataview is working"
   ---

   # Test Insight

   Just a placeholder.
   ```

2. Open `60_MOCs/Scenarios/Self-Review.md` in reading view. The table should now show one row with your test insight.
3. Open `60_MOCs/Scenarios/Code-Tips.md`. Same insight should appear there too — that's the cross-cutting query in action.
4. Delete the test file when satisfied.

### Quick reference: Dataview query syntax

The MOCs use these patterns. You can copy them when building your own queries.

**List all insights for one scenario:**

````markdown
```dataview
LIST summary
FROM "40_Insights"
WHERE contains(scenarios, "self-review")
SORT date DESC
```
````

**Table with multiple columns:**

````markdown
```dataview
TABLE summary AS "Insight", projects AS "Projects", date AS "Date"
FROM "40_Insights"
WHERE contains(scenarios, "code-tips")
SORT date DESC
```
````

**Filter by tag AND scenario (e.g., React tips only):**

````markdown
```dataview
LIST summary
FROM "40_Insights"
WHERE contains(scenarios, "code-tips") AND contains(tags, "react")
```
````

**Insights about a specific person:**

````markdown
```dataview
LIST summary
FROM "40_Insights"
WHERE contains(people, "[[Sarah]]")
SORT date DESC
```
````

**All insights from a project:**

````markdown
```dataview
LIST summary
FROM "40_Insights"
WHERE contains(projects, "[[ProjectName]]")
```
````

### Common gotchas

- **Empty results**: check that the frontmatter field is at the top level (not nested) and that values match exactly. `scenarios: self-review` won't match a query for `["self-review"]` — use list form: `scenarios: [self-review]`.
- **Query renders as plain code**: you're in editing/source view. Switch to reading view (Ctrl/Cmd+E) or live preview.
- **Slow queries on a big vault**: scope `FROM` to a specific folder (`FROM "40_Insights"`) instead of querying everything.

### Skipping Dataview

If you decide not to use Dataview, the MOCs still work — but they become hand-maintained link lists instead of auto-populating queries. You'll need to add a link manually each time you create an insight. Workable for a small vault, painful at scale.

## 4. Install the Obsidian Local REST API plugin and obsidian-mcp

The wrap-up and insight-extraction skills read and write vault files through the **obsidian-mcp**, which is backed by the **Obsidian Local REST API** community plugin. Without this, the skills cannot access your vault — they will stop and tell you the mcp is not connected rather than fall back to the file system.

### 4a. Install the Obsidian Local REST API plugin

1. In Obsidian: **Settings → Community plugins → Browse**.
2. Search **Local REST API**.
3. Click the result → **Install** → **Enable**.
4. Go to **Settings → Local REST API**.
5. Note the **API key** shown — you'll need it in the next step.
6. Keep the default port (`27123`) unless it conflicts with something else on your machine.
7. Leave the plugin enabled whenever you plan to use Claude with your vault.

The plugin exposes a local HTTP server that lets external tools (like obsidian-mcp) read and write your notes securely without needing access to the raw file system.

### 4b. Install and configure obsidian-mcp

obsidian-mcp is the MCP server that wraps the Local REST API so Claude Code can call it as a tool.

```bash
# Install via npm (requires Node.js 18+)
npm install -g obsidian-mcp
```

Then add it to your Claude Code MCP config (`~/.claude/mcp.json` or your project-level `.mcp.json`):

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "obsidian-mcp",
      "args": [],
      "env": {
        "OBSIDIAN_API_KEY": "<your-api-key-from-step-4a>",
        "OBSIDIAN_BASE_URL": "http://localhost:27123"
      }
    }
  }
}
```

Replace `<your-api-key-from-step-4a>` with the key from the plugin settings.

### 4c. Verify the connection

Start a Claude Code session inside your vault and run:

> "Use the obsidian-mcp to read `_meta/Vault-Conventions.md` and tell me the first line."

If Claude returns the content, the mcp is wired up correctly. If it errors, check that:

- Obsidian is open and the Local REST API plugin is enabled.
- The API key in your mcp config matches the one in plugin settings.
- The port in `OBSIDIAN_BASE_URL` matches the plugin's configured port.

## 5. Other recommended Obsidian settings

- **Settings → Files & Links → New link format**: `Relative path to file` (keeps links portable when folders move).
- **Settings → Files & Links → Default location for new notes**: `In the current folder`.
- **Settings → Editor → Strict line breaks**: off.

## 6. Fill in `_meta/Me.md`

Open `_meta/Me.md` and replace the placeholder sections with your own info. This file is referenced by every project's CLAUDE.md, so it's the foundation of how Claude works with you.

Fast path: ask Claude to "interview me for my Me.md" and it'll ask 5–8 questions and generate the file.

## 7. Set up the skills

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

## 8. Start your first project

```bash
mkdir -p 20_Projects/MyFirstProject/{sessions,decisions,artifacts}
cp _templates/CLAUDE.md 20_Projects/MyFirstProject/CLAUDE.md
cp _templates/project-index.md 20_Projects/MyFirstProject/_index.md
```

Open `CLAUDE.md` and `_index.md`, fill in goals and current focus, and you're ready to work.

## 9. Test the wrap-up skill

Have a working session with Claude inside the project folder, then say **"wrap up this session"**. It should:

1. Generate a session summary using the project-session template.
2. Save it to `20_Projects/MyFirstProject/sessions/YYYY-MM-DD_<slug>.md`.
3. Append a link in `_index.md` under "Recent sessions".
4. Reply with the saved file path.

## 10. Schedule insight extraction

Decide on a cadence (weekly is a good default). At your chosen time:

1. Glance at the inbox and triage anything new.
2. Run the insight-extraction skill: "extract insights from new notes".
3. Review the proposed insights — accept, edit, or reject each one.

## Troubleshooting

- **obsidian-mcp not connected / skill stops immediately**: make sure Obsidian is open and the Local REST API plugin is enabled. The plugin only serves requests while Obsidian is running.
- **API key mismatch error**: open **Settings → Local REST API** in Obsidian and copy the key again — it regenerates if you reinstall the plugin.
- **Port conflict**: if `27123` is taken, change it in the plugin settings and update `OBSIDIAN_BASE_URL` in your mcp config to match.
- **`obsidian-mcp` command not found**: make sure you ran `npm install -g obsidian-mcp` and that your global npm bin directory is on your `PATH` (`npm bin -g` shows the path).
- **Frontmatter doesn't render in Obsidian**: enable **Settings → Editor → Properties in document → Visible**.
- **Dataview query shows nothing**: check that your insight files have a top-level `scenarios:` field (not nested under another key) and the values match what's in `_meta/Scenarios.md`.
- **Wrap-up keeps saving to the wrong folder**: clarify the routing rules in your project's `CLAUDE.md` or in `_meta/Vault-Conventions.md`.
