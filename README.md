# Claude Code Skills

Session management and cross-tool skills for [Claude Code](https://claude.com/claude-code) — persistent memory, continuity across sessions, and interop with other AI coding tools.

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

### `/devils-advocate` — Cross-Model Adversarial Critique

Sends your work to Google Gemini for an outside critique. Takes a campaign plan, positioning statement, blog draft, chunk of code, or any other artifact and gets a structured teardown from a model in a different lineage.

- **Steelmans first** — builds the strongest honest case for the work before attacking it
- **Structured output** — specific weaknesses with location, problem, why it matters, and suggested fix
- **Verdict** — returns SHIP, SHIP WITH FIXES, REWORK, or KILL with reasoning
- **Context-aware** — pass supporting files (positioning docs, audience profiles, product truth) so the critique is grounded in your actual strategy

Requires a `GOOGLE_AI_STUDIO_API_KEY` in your `.env` file. No other dependencies.

### `/sync-skills` — Claude → Codex Skill Sync

Keeps your Claude Code skills available in Codex. Prompts for scope, then syncs:

- **Project-level** — hardlinks `.claude/skills/` → `.agents/skills/` in your repo. Codex can't follow symlinks reliably, so hardlinks are used instead. Edits in either location update both.
- **User-level** — copies `~/.claude/skills/` → `~/.codex/skills/` with a `.synced-from-claude` marker file to track provenance. Skills you create directly in `~/.codex/skills/` are preserved.

Detects and skips skills managed by their own installer (e.g. [gstack](https://github.com/garrytan/gstack)'s `./setup --host codex`) so there are no collisions. Marker file approach borrowed from [ariccb/sync-claude-skills-to-codex](https://lobehub.com/skills/ariccb-sync-claude-skills-to-codex-sync-claude-skills-to-codex).

## Installation

Copy the skills into your project's `.claude/skills/` directory:

```
.claude/skills/
├── brief/
│   └── SKILL.md
├── start/
│   └── SKILL.md
├── reflect/
│   └── SKILL.md
├── sync-skills/
│   └── SKILL.md
└── devils-advocate/
    ├── SKILL.md
    └── scripts/
        └── challenge.py
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
