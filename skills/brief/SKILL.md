---
name: brief
version: 2.0.0
description: "/brief - Write session brief, commit, and push. Run at end of session to checkpoint all work."
---

# Session Brief

Run this at the end of a session to capture context and checkpoint all work to GitHub.

## Steps

### 1. Review Existing Brief

Read `/brief/session-brief.md` to understand what was there from last session.

### 2. Write the New Brief

Overwrite `/brief/session-brief.md` with a comprehensive brief covering:

- **Date, time (EST), and session number** at the top
- Important progress made in the current session
- Key decisions and architectural changes
- Unfinished tasks and next steps
- Technical details that need to be preserved
- Any context that would be lost between sessions

This is NOT about brevity — be as thorough as needed to retain all important context.

### 3. Commit and Push

After writing the brief:

1. Run `git status` to see all changes (staged + unstaged + untracked)
2. Run `git log --oneline -3` to match commit message style
3. Stage all relevant files (`git add` — use specific paths, not `-A`)
4. Commit with message format: `Session [N]: [1-2 sentence summary of session work]`
5. Push to origin: `git push`

**Commit rules:**
- Do NOT stage files that look like secrets (`.env`, credentials, tokens)
- If there are uncommitted changes unrelated to this session, ask the user before including them
- If push fails (e.g., remote is ahead), stop and ask the user — do not force push

## When to Use

- At the end of significant sessions
- When important decisions have been made
- Before ending a session where critical details might be lost
- When the user explicitly requests `/brief`

## Read-Only Mode

When asked to "read the brief" (typically at session start via `/start`), just read the file without modifying it. Do not commit or push in read-only mode.
