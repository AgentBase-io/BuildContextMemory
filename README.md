# Build Context Memory

A tiny system that keeps AI-assisted builds from falling apart across sessions, tools, and context-window resets.

## The Problem (Why this exists)

When building with AI tools, project context gets fragmented:

- context windows run out
- credits get exhausted
- you switch tools (Cursor, Copilot, ChatGPT, Claude, etc.)
- sessions restart with no memory

Result: decisions get lost, work gets duplicated, and build quality crumbles.

## The Solution: a 2-file operating system

This repo uses two required files:

1. **`AI_Starter.md`**
   - The behavior contract for any AI agent.
   - Defines what to read first, how to behave, and what to update.
2. **`Build-Context-Memory.json`**
   - The living memory log.
   - Stores project identity plus append-only session objects.

### How they work together

- `AI_Starter.md` is the **brain** (instructions and rules).
- `Build-Context-Memory.json` is the **memory** (state over time).

Neither is useful alone:
- Brain without memory forgets.
- Memory without behavior rules drifts and becomes unreliable.

## Getting Started (Step by step)

1. Clone/copy this repo pattern into your project.
2. Keep these files in your project root:
   - `README.md`
   - `AI_Starter.md`
   - `Build-Context-Memory.json`
   - `PRD.md`
   - `ARCHITECTURE.md`
   - `DEV_PLAN.md`
3. Fill in `PRD.md`, `ARCHITECTURE.md`, and `DEV_PLAN.md` for your real project.
4. Fill in `project_identity` in `Build-Context-Memory.json`.
5. At the start of **every** AI session, paste the exact prompt below.
6. After every meaningful change, append a new session object in `Build-Context-Memory.json`.

### Exact prompt to paste at the start of every AI session

```text
Read and follow /AI_Starter.md before doing anything.
Then read /PRD.md, /ARCHITECTURE.md, /DEV_PLAN.md, and the latest session in /Build-Context-Memory.json.
Confirm what the next step is from `next_steps`, ask any missing clarifying questions, then proceed.
After every meaningful update, append a new session object to `sessions` in /Build-Context-Memory.json (append-only, never modify old sessions, never modify `project_identity`).
```

## Real example: filled session object

Use this as a reference for what "good memory" looks like:

```json
{
  "session_id": "session_002",
  "timestamp": "2026-05-08T16:20:00Z",
  "ai_agent": "GitHub Copilot Task Agent",
  "current_phase": "Foundation Setup",
  "progress": {
    "completed": [
      "Defined initial project memory schema",
      "Created AI starter rules for all agents",
      "Added README with usage instructions"
    ],
    "in_progress": [
      "Drafting project-specific PRD content"
    ],
    "blocked": []
  },
  "last_session_summary": "Initialized the repository with starter memory and behavior contract files.",
  "changes_made": [
    "Created README.md",
    "Normalized AI_Starter.md naming",
    "Filled Build-Context-Memory.json with baseline project identity"
  ],
  "file_module_map": [
    {
      "file": "README.md",
      "purpose": "Explains problem, setup, rules, and philosophy",
      "last_modified": "2026-05-08T16:20:00Z"
    },
    {
      "file": "Build-Context-Memory.json",
      "purpose": "Stores append-only project state and session log",
      "last_modified": "2026-05-08T16:20:00Z"
    }
  ],
  "decisions_log": [
    {
      "decision": "Use append-only sessions in a JSON file",
      "reason": "Prevents accidental loss of historical build context",
      "alternatives_considered": "Single mutable status document"
    }
  ],
  "known_issues": [
    {
      "issue": "PRD template still generic",
      "severity": "low",
      "status": "open"
    }
  ],
  "errors_encountered": [],
  "next_steps": [
    "Fill PRD.md with concrete product scope",
    "Define module boundaries in ARCHITECTURE.md",
    "Break work into milestones in DEV_PLAN.md"
  ],
  "notes": "Session format validated and ready for handoff to any AI tool."
}
```

## AI rules summary

| Rule | What the AI must do |
|---|---|
| Read first | Read `AI_Starter.md`, `PRD.md`, `ARCHITECTURE.md`, `DEV_PLAN.md`, and latest JSON session before work |
| Clarify scope | Ask questions if task/phase/constraints are unclear |
| Stay in scope | Build only what PRD + plan define unless explicitly approved |
| Work incrementally | Make small reviewable updates and summarize logical units |
| Log decisions/errors | Record notable decisions, issues, and error resolutions in session object |
| Append-only memory | Add new session objects only; never edit/delete old sessions |
| Protect identity | Never modify `project_identity` once initialized |
| Always set next steps | Never end a session with empty `next_steps` |

## Why this approach works (Philosophy)

AI is strong at local execution but weak at persistent project memory across sessions.
This system separates concerns:

- **Behavior control** (`AI_Starter.md`) keeps all agents aligned.
- **State continuity** (`Build-Context-Memory.json`) preserves history and intent.

That combination turns stateless AI chats into a continuous, auditable development loop.

When context resets, the project does not.
