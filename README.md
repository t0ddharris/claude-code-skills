# Claude Code Skills

Session management skills for [Claude Code](https://claude.com/claude-code) — designed to give AI-assisted workflows persistent memory and continuity across sessions.

## The Problem

Desktop AI tools are getting better at persistence — memory features, project files, custom instructions. But it's still persistence bolted onto a chat interface. You're attaching context to a conversation.

Claude Code operates inside your project. It reads your file structure, follows your frameworks, carries memory across sessions, and writes output directly into organized folders. These skills lean into that by solving one of the biggest practical problems: **session continuity**.

Without a handoff system, every new Claude Code session starts with "where were we?" — re-reading files, re-establishing context, figuring out what's done and what's open. And without a feedback loop, every correction you make in conversation disappears the moment the session ends. These skills eliminate both problems.

## Skills

### `/brief` — Session Close

Run at the end of a session. Writes a comprehensive handoff document capturing:

- Progress made in the current session
- Key decisions and changes
- Unfinished tasks and next steps
- Technical details that would be lost between sessions

Then commits everything to git and pushes. Your work is checkpointed and the next session has full context.

### `/start` — Session Open

Run at the beginning of a session. Reads the last session's brief, checks git status, and delivers a concise summary:

- What happened last session
- Open items prioritized by urgency
- Any uncommitted changes
- Current session number

No re-reading, no "where were we?" — just orientation and go.

### `/reflect` — Skill File Improvement

Run at the end of a session (or automatically as part of `/brief`). Scans the conversation for corrections, approvals, and validated patterns, then proposes updates to whichever skill files were used during the session.

- Extracts explicit corrections ("don't do X," "always use Y") as high-confidence rules
- Captures quietly approved approaches as medium-confidence patterns
- Filters out one-time contextual decisions that don't generalize
- Proposes additions to a `## Learnings` section at the bottom of each affected skill file
- Graduates stable learnings into the main skill body after 5+ sessions without contradiction
- Nothing is written without explicit user approval

Every correction you make in conversation becomes a durable rule instead of evaporating at session end. Skill files improve from use the way we improve from experience.

## Installation

Copy the skills into your project's `.claude/skills/` directory:

```
.claude/skills/
├── brief/
│   └── SKILL.md
├── start/
│   └── SKILL.md
└── reflect/
    └── SKILL.md
```

Create a `/brief/` directory in your project root for the session brief file:

```
brief/
└── session-brief.md
```

Then use `/brief` at the end of sessions and `/start` at the beginning. `/reflect` runs automatically as part of `/brief` when both skills are installed, or you can invoke it standalone at any time.

## How It Fits

These skills assume you're using Claude Code on a git-tracked project. The `/brief` skill commits and pushes, so your session history lives in version control alongside your work. The `/start` skill reads the brief and git state to orient the next session.

They work with any type of project — software, marketing, writing, research — anything where you want AI-assisted work to compound across sessions instead of starting fresh every time.
