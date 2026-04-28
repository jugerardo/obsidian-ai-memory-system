# Obsidian AI Memory System

A structured Obsidian vault for capturing your work with AI tools — conversations, project sessions, code sessions — and turning it into a queryable knowledge base over time.

Built around a simple idea: **conversations are logs, but insights are atoms**. Logs go in dated session folders. Atomic insights live in a flat insights folder, tagged by *scenario* (self-review, code-tips, working-with-people, etc.) so you can pull cross-cutting context whenever you need it.

## The problem this solves

If you've used ChatGPT projects, you've probably hit this wall: every project becomes its own silo. When performance review time comes around and you want to draft a self-review using insights from across a year of work — the context doesn't exist. Every conversation lives in its own bubble.

This system fixes that by:

1. Capturing all your AI work in one vault, regardless of which tool produced it.
2. Extracting durable insights as standalone, atomic notes.
3. Tagging insights by *scenario* (use case), so you can pull cross-cutting bundles on demand.
4. Letting Claude (or any AI) read the whole vault as context for new work.

## What's in this repo

This repo is a **template** — folder structure, note templates, skills, conventions. Clone it, set up Obsidian, and you have a working memory system. Your actual notes live in your private copy, not here.

```
.
├── README.md                  ← You are here
├── SETUP.md                   ← Step-by-step setup including Dataview
├── _meta/
│   ├── Me.md                  ← Profile template (your fill-in)
│   ├── Vault-Conventions.md   ← How this vault is organized
│   └── Scenarios.md           ← Controlled vocabulary for cross-cutting tags
├── _templates/
│   ├── CLAUDE.md              ← Drop into each project folder
│   ├── conversation.md
│   ├── project-session.md
│   ├── code-session.md
│   ├── insight.md
│   ├── project-index.md
│   ├── skill-wrap-up.md       ← The "wrap up this session" skill
│   └── skill-insight-extraction.md  ← The "extract insights" skill
├── 00_Inbox/                  ← Untriaged captures
├── 10_Conversations/          ← Standalone AI chats
├── 20_Projects/               ← Multi-session project work
├── 30_Code_Sessions/          ← Standalone coding sessions
├── 40_Insights/               ← Atomic notes — the knowledge layer
├── 50_References/             ← External docs
├── 60_MOCs/                   ← Maps of Content (entry points)
└── 90_Archive/
```

## How it works (the mental model)

```
                ┌──────────────┐
   Raw input → │ 00_Inbox     │ ← You drop exports/captures here
                └──────┬───────┘
                       │ (you triage)
                       ▼
        ┌──────────────────────────────────┐
        │ 10_Conversations / 20_Projects /  │  ← Logs of what happened
        │ 30_Code_Sessions                  │
        └──────────────┬────────────────────┘
                       │ (insight-extraction skill)
                       ▼
                ┌──────────────┐
                │ 40_Insights  │ ← Atomic, scenario-tagged knowledge
                └──────┬───────┘
                       │ (queries)
                       ▼
                ┌──────────────┐
                │ 60_MOCs      │ ← Use-case views (self-review, code-tips, …)
                └──────────────┘
```

### The two skills

Both skills communicate with your vault through the **obsidian-mcp** (backed by the Obsidian Local REST API plugin). All file reads, creates, and updates go through the MCP — Claude never touches vault files directly via the file system. This means the mcp must be running whenever you invoke a skill.

- **Wrap-up skill** — at the end of any session, you say "wrap up" and Claude generates a structured session summary, saves it to the right folder (project sessions / code sessions / standalone conversations), and updates the project's `_index.md` if applicable.
- **Insight-extraction skill** — runs over notes flagged `status: raw`, identifies atomic insights worth keeping, creates them in `40_Insights/` with scenario tags, and links them back to the source. Run this on a schedule (weekly is a good cadence).

### Scenarios

The load-bearing concept. Scenarios are tags that answer the question *"when will I want this insight back?"*

Starter scenarios include:

- `self-review` — accomplishments and growth, for performance reviews
- `code-tips` — durable coding patterns
- `code-troubleshooting` — bugs/issues with non-obvious fixes
- `design-portfolio` — work suitable for portfolio/case studies
- `stakeholder-feedback` — feedback received from PMs, leadership, customers
- `working-with-people` — communication preferences per person
- `decisions` — significant decisions with rationale
- `lessons-learned` — postmortems, things you'd do differently

Each insight can belong to multiple scenarios. New scenarios get added to `_meta/Scenarios.md` as patterns emerge.

### Importing from other AI tools

Drop ChatGPT/Gemini/etc. exports into `00_Inbox/`, then triage:

1. Decide which folder the note belongs to (conversation / project session / etc.)
2. Move it
3. Add frontmatter with `status: raw`
4. Run the insight-extraction skill on a schedule to harvest insights

A future improvement is automated import that handles frontmatter and routing — see [Roadmap](#roadmap).

## Getting started

See [SETUP.md](./SETUP.md) for full instructions, including:

- Cloning the template into your private vault
- Installing Obsidian + Dataview plugin
- Installing the Obsidian Local REST API plugin and obsidian-mcp (required for the skills)
- Setting up the wrap-up and insight-extraction skills
- Running the Me.md interview to seed your profile

## Customizing for yourself

The `_meta/Me.md` file is the most important customization — it tells Claude how you like to work, what you know well, and what to avoid. Every CLAUDE.md template references it.

Beyond that:

- Edit `_meta/Scenarios.md` to add or remove scenarios as your needs evolve
- Tweak templates in `_templates/` — they're starting points, not gospel
- Build your own MOCs in `60_MOCs/` for any topic that earns its own hub

## Roadmap

Things planned for future iterations:

- Automated import from ChatGPT/Gemini export formats (parse the JSON, generate frontmatter, route to the right folder)
- Insight deduplication during extraction
- Auto-suggestion of new scenarios when patterns emerge
- A "warm start" command that loads the most relevant scenarios into context when you begin a new project

Contributions and forks welcome.

## License

MIT — see [LICENSE](./LICENSE).
