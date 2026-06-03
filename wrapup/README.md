# The `/wrapup` skill — what it is and how to set it up

A personal-productivity slash command for Claude Code. It closes out a working session by updating the right memory files and summarizing what happened. Think of it as the "end of session" ritual, automated.

> **How to use this file:** read the first two sections to understand what you're getting. Then either set it up yourself using the template at the bottom, or paste this whole file into Claude Code and say *"set this up for me"* — Claude will follow the **Setup instructions for Claude** section and walk you through it.

---

## What it does

When you type `/wrapup` at the end of a Claude Code session, Claude:

1. **Reviews the session** — what was discussed, what todos were completed, what decisions were made, what new info came up.
2. **Updates the right "department" memory file** — you keep separate memory files per area of your work (e.g. Product, Sales, Content, Ops, or whatever matches your life). Claude identifies which one this session belonged to and updates it with what happened and what's next.
3. **Updates the top-level memory file** — only if top-level facts changed (new client, pricing change, strategic shift).
4. **Presents a summary** — done / decisions / carries over / files updated.

The point: you never lose context between sessions, and memory stays organized by topic instead of one growing log.

---

## How slash commands work in Claude Code

- Slash commands are markdown files in `.claude/commands/` (project-level) or `~/.claude/commands/` (global).
- The filename becomes the command: `wrapup.md` → `/wrapup`.
- The file content is the prompt Claude executes when you invoke it. No special syntax — just clear instructions in plain English.
- Project-level commands only work inside that project; global ones work everywhere.

---

## What the system depends on

The skill itself is tiny. The value comes from the **filing system** it operates on. You need:

### 1. A memory directory with "department" files
A folder (e.g. `memory/`) with one file per stable area of your work:
- `MEMORY.md` — the index / top-level overview (always auto-loaded into context)
- `product.md`, `sales.md`, `content.md`, `ops.md`, `clients.md` — one per "department"

Name these whatever matches your work. Categories must be **stable** — don't reinvent them each session.

### 2. A `CLAUDE.md` at the project root
Claude Code reads `CLAUDE.md` automatically as always-on instructions. Document your filing system there so Claude knows which file to update for what. `/wrapup` relies on this context.

At minimum, `CLAUDE.md` should describe:
- What departments/categories exist and what each covers
- Where memory files live (absolute path)
- Any "one source of truth per topic" rules so Claude doesn't duplicate

---

## Setup instructions for Claude

If the user pasted this file in and asked you to set up `/wrapup`, follow these steps:

### Step 1 — Ask the user 4 questions
Ask all four in one message, then wait for answers:

1. **Where's your project root?** (the directory you want this scoped to — Claude Code will read the `CLAUDE.md` from there)
2. **What "departments" or categories should we use?** (3–6 stable areas — e.g. Product / Sales / Ops, or Work / Side-projects / Personal). If they're unsure, suggest a starter set based on what you can infer from their project.
3. **Where should memory files live?** (default: `~/.claude/projects/<project-slug>/memory/` — Claude Code's per-project memory location. Or somewhere inside the project itself.)
4. **Project-level or global skill?** (project = only works in this directory; global = works everywhere. Default to project.)

### Step 2 — Create the memory directory and seed files
Create one file per department, plus `MEMORY.md`. Each department file starts with a short header:

```markdown
# <Department Name>

## Current state
(empty — populate as you work)

## Active threads
(empty)

## Last updated
<today's date>
```

`MEMORY.md` is the index — list each department file with a one-line description, and add any top-level facts the user mentioned (role, current phase, key clients, etc.).

### Step 3 — Update or create `CLAUDE.md`
At the project root, add a section that documents the filing system:

```markdown
## Memory & session protocol

Memory files live at: <absolute path>

Departments:
- product.md — <what it covers>
- sales.md — <what it covers>
- (etc.)

End each session with `/wrapup` to update the relevant memory file.
```

If `CLAUDE.md` already exists, append this section. Don't overwrite.

### Step 4 — Write the skill file
Save the template at the bottom of this document to:
- **Project-level:** `<project root>/.claude/commands/wrapup.md`
- **Global:** `~/.claude/commands/wrapup.md`

Replace `<MEMORY_PATH>` with the absolute path from Step 2, and replace the example department list with the user's actual departments.

### Step 5 — Confirm & test
Tell the user:
- What got created and where
- That they can test it by typing `/wrapup` after their next working session
- That the skill will only feel useful once memory files have a few sessions of content in them — first wrapup will be sparse

---

## The skill template

Save this to `<project>/.claude/commands/wrapup.md` (or `~/.claude/commands/wrapup.md` for global). Replace `<MEMORY_PATH>` and the department list with the user's actual setup.

```markdown
# Session Wrapup

Close the current session by updating all relevant files. Follow these steps exactly.

---

## Step 1: Review What Happened

Look at:
- What was discussed/completed in this session
- Any todos from this session — check their status
- Any decisions made
- Any new information learned (client updates, pricing changes, etc.)

---

## Step 2: Update Department Memory

Identify which department this session belonged to and update the corresponding memory file.

Memory files are at: `<MEMORY_PATH>`

- `product.md` — if analysis/delivery/code work happened
- `sales.md` — if pipeline/outreach/client comms happened
- `content.md` — if LinkedIn/SEO/engagement happened
- `ops.md` — if admin/accounting/contracts happened
- `clients.md` — if any client status changed

Update the "Last updated" date at the bottom of each changed file.

If the session spans multiple departments, update each one — but keep entries scoped to that department's concern.

---

## Step 3: Update MEMORY.md (only if needed)

Only update `MEMORY.md` if top-level facts changed:
- New client acquired or lost
- Pricing/offer changed
- Phase shifted
- Major strategic decision

Don't log routine session activity here — that belongs in department files.

---

## Step 4: Summary

Present to the user:

## Session Summary

### Done
- [what was completed]

### Decisions Made
- [any decisions]

### Carries Over
- [what's still open]

### Files Updated
- [list of files that were updated]
```

---

## Iterating

The skill is just a markdown file — edit it any time. Common tweaks once you've used it for a few weeks:
- Add a step that updates a weekly planning file (e.g. `This Week.md`)
- Add a step that flags stale entries (no update in 30+ days)
- Add a department, or merge two that overlap
- Tighten the summary format if it's getting noisy

Build the system first. The command just automates the boring part of keeping it current.
