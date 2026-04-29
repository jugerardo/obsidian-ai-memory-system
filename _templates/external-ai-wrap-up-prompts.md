# External AI wrap-up prompts

Use these prompts at the end of a session in ChatGPT, Gemini, or any other AI tool to generate a session summary already formatted for this vault. Copy the output and drop it in `00_Inbox/` — the automated import skill will pick it up from there.

Paste the relevant prompt at the end of your conversation, then copy the full markdown output the AI returns.

---

## Conversation (general discussion, no code)

````
Summarize this conversation as a structured session note using the following markdown template exactly. Fill in every section based on what we actually discussed — don't leave placeholder text. Output only the markdown, no preamble.

---
type: conversation
date: {{today's date in YYYY-MM-DD}}
source: {{chatgpt | gemini | other}}
status: raw
tags: []
summary: "<one sentence describing what this conversation was about>"
---

# {{A short descriptive title for this conversation}}

## Context

{{Why I started this conversation and what I was trying to figure out.}}

## What we discussed

{{The main thread of the conversation. Concise — a log, not a transcript.}}

## Key takeaways

{{The 1–3 things worth remembering. One bullet each.}}

-

## Candidate insights

{{Anything atomic and durable that might deserve its own note later. One bullet per candidate, one-line description each. Be generous — a separate process will filter. If nothing qualifies, write "none".}}

-

## Follow-ups

{{What I want to do next, if anything. If nothing, write "none".}}

-

## Source links

{{Link or reference to this conversation if available (e.g. a ChatGPT share link). Otherwise leave blank.}}

-
````

---

## Code session (debugging, building, exploring a library)

````
Summarize this coding session as a structured session note using the following markdown template exactly. Fill in every section based on what we actually worked on — don't leave placeholder text. Output only the markdown, no preamble.

---
type: code-session
date: {{today's date in YYYY-MM-DD}}
source: {{chatgpt | gemini | other}}
status: raw
project: ""
tags: [code]
languages: [{{comma-separated list of languages used, e.g. python, typescript}}]
frameworks: [{{comma-separated list of frameworks used, or leave empty}}]
summary: "<one sentence describing what this session accomplished>"
---

# {{today's date in YYYY-MM-DD}} — {{A short descriptive title}}

## Goal

{{What I was trying to do or fix at the start of this session.}}

## Approach

{{How I went about it — hypotheses, what I tried, dead ends, what finally worked.}}

## Key code / commands

{{The most useful snippet or command from the session. Just one — don't paste entire files.}}

```language
// paste snippet here
```

## What worked

{{The fix or technique that resolved the goal. One bullet per item.}}

-

## What didn't

{{Dead ends worth remembering so I don't retry them. One bullet per item.}}

-

## Errors encountered (and how I diagnosed them)

{{The error message → root cause → fix chain. One bullet per error. This is the most valuable part for future reference.}}

-

## Candidate insights

{{Durable lessons from this session — coding tricks, non-obvious fixes, tool learnings. One bullet per candidate. If nothing qualifies, write "none".}}

-

## Follow-ups

{{What's still open or next. If nothing, write "none".}}

-
````

---

## Project session (multi-session project work)

````
Summarize this project work session as a structured session note using the following markdown template exactly. Fill in every section based on what we actually worked on — don't leave placeholder text. Output only the markdown, no preamble.

---
type: project-session
date: {{today's date in YYYY-MM-DD}}
project: "[[{{Project name}}]]"
source: {{chatgpt | gemini | other}}
status: raw
tags: []
summary: "<one sentence describing what happened in this session>"
---

# {{today's date in YYYY-MM-DD}} — {{A short descriptive title for this session}}

## Goal for this session

{{What we set out to accomplish.}}

## What happened

{{Substantive work, in order. Include decisions, code snippets, and diagrams inline as needed. This is the main body — be thorough.}}

## Decisions made

{{Significant calls made this session. One bullet per decision. If nothing major, write "none".}}

-

## Outcomes

- **Completed**: {{what got done}}
- **In progress**: {{what's mid-flight}}
- **Blocked**: {{what's stuck and why}}

## Next steps

{{What's queued up. One bullet per item.}}

-

## Candidate insights

{{Atomic, durable lessons that might deserve their own notes. One bullet per candidate. If nothing qualifies, write "none".}}

-

## Artifacts

{{Links to any files, docs, or outputs created. If none, write "none".}}

-
````

---

## Tips for better output

- **Run the prompt at the very end of your session** — the AI has the full conversation in context at that point.
- **If the output is missing sections**, paste the prompt again and add: "Make sure to fill in the [section name] section — don't skip it."
- **For the `source:` field**, replace the placeholder with `chatgpt`, `gemini`, or whatever tool you used.
- **For `tags:`**, you can leave it empty — the import and extraction steps will fill it in.
- **Candidate insights are the most important section** to check before saving. The AI tends to be conservative; feel free to add bullets for things you know are worth keeping.
