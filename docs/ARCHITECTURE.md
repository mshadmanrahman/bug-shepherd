# Architecture

Bug Shepherd is a set of Claude Code slash commands, agent definitions, and templates that form a complete bug triage operations system. It has no runtime dependencies beyond Claude Code itself.

## Design Principles

### 1. No Code Knowledge Required
The primary user is a PM, designer, or QA engineer who doesn't live in the codebase. Every command works without reading source code. The system reproduces bugs by navigating the live site, not by running tests.

### 2. Institutional Memory
Every session captures lessons. These lessons are loaded at the start of the next session. Over time, the system builds a knowledge base of bug patterns, investigation heuristics, and common pitfalls specific to YOUR project.

### 3. Human Gates at Risky Transitions
Automation handles the tedious parts (syncing, reproducing, categorizing). Humans make the judgment calls (what to cancel, what to escalate, what to fix). This split was designed after a real incident where automated cancellation went wrong.

### 4. Tracker-Agnostic Core
The core workflow is the same regardless of bug tracker. Tracker-specific logic lives in adapter files that translate between Bug Shepherd's internal model and the tracker's API.

## System Components

```
┌─────────────────────────────────────────────────┐
│                   User (PM/Dev)                  │
├─────────────────────────────────────────────────┤
│              Slash Commands (5)                   │
│  /shepherd-sync  → Backlog synchronization       │
│  /shepherd-start → Session setup                 │
│  /shepherd-triage → Mass reproduction check      │
│  /shepherd-review → Quality gate                 │
│  /shepherd-learn  → Lesson capture               │
├─────────────────────────────────────────────────┤
│           Configuration Layer                     │
│  triage.config.yaml → All project settings       │
│  adapters/{tracker}.md → Tracker-specific logic  │
├─────────────────────────────────────────────────┤
│           Persistent State                        │
│  backlog-live.md → Synced bug inventory          │
│  learning-log.md → Institutional memory          │
│  triage-log-{date}.md → Triage session results   │
│  quality-gate.md → Pre-push checklist            │
├─────────────────────────────────────────────────┤
│           Agent Layer                             │
│  reproduction-checker.md → Parallel bug checker  │
├─────────────────────────────────────────────────┤
│           External Services                       │
│  Bug Tracker (Jira/Linear/GitHub) via MCP        │
│  Live Site via Playwright MCP                    │
└─────────────────────────────────────────────────┘
```

## Data Flow

### Sync Flow
```
Bug Tracker API → /shepherd-sync → backlog-live.md
```

### Triage Flow
```
backlog-live.md → /shepherd-triage → parallel agents → live site (Playwright)
                                   → categorized results → HUMAN REVIEW
                                   → approved actions → Bug Tracker API
                                   → triage-log-{date}.md
```

### Fix Flow
```
backlog-live.md → /shepherd-start → session context + learning log
               → investigation → proposed fix → HUMAN APPROVAL
               → implementation → /shepherd-review → quality gate
               → HUMAN APPROVAL → push + PR → /shepherd-learn
               → learning-log.md (appended)
```

## File Relationships

```
triage.config.yaml
  ├── read by: all 5 commands
  ├── references: adapters/{tracker}.md
  └── contains: safety rules, team settings, review rules

backlog-live.md
  ├── written by: /shepherd-sync
  ├── read by: /shepherd-start, /shepherd-triage
  └── updated by: /shepherd-learn, /shepherd-triage

learning-log.md
  ├── written by: /shepherd-learn
  ├── read by: /shepherd-start, /shepherd-review
  └── grows over time (append-only)

triage-log-{date}.md
  ├── written by: /shepherd-triage
  ├── read by: /shepherd-sync (to mark triaged bugs)
  └── one per triage session

quality-gate.md
  ├── reference checklist
  └── read by: /shepherd-review
```

## Parallel Agent Architecture

The mass triage command (`/shepherd-triage`) launches multiple agents in parallel:

```
/shepherd-triage (orchestrator)
  ├── Pre-filter (safety rules)
  ├── Split bugs into batches
  ├── Launch Agent 1 (bugs 1-6)  ──→ Playwright → live site
  ├── Launch Agent 2 (bugs 7-12) ──→ Playwright → live site
  ├── Launch Agent 3 (bugs 13-18) ─→ Playwright → live site
  ├── Launch Agent 4 (bugs 19-24) ─→ Playwright → live site
  └── Launch Agent 5 (bugs 25-30) ─→ Playwright → live site
      │
      ├── Collect results
      ├── Categorize (auto-cancel / human review / reproduced / unknown)
      ├── Present to user (HUMAN GATE)
      └── Execute approved actions
```

Each agent uses the `reproduction-checker.md` definition and operates independently. Agents don't communicate with each other; the orchestrator merges their results.

## Configuration Precedence

1. `triage.config.yaml` — primary configuration
2. Adapter defaults — tracker-specific defaults
3. Hardcoded defaults — minimal safe defaults in commands

If a config value is missing, commands use safe defaults (e.g., 30 bugs per triage, 5 parallel agents, 90-day activity window).

## Security Considerations

- **No secrets in config:** The config file contains no API keys or tokens. Authentication is handled by MCP servers and the gh CLI.
- **Read-only reproduction:** Agents only navigate and screenshot. They never submit forms, click destructive actions, or modify data on the live site.
- **Human gates:** All tracker write operations (cancel, comment, assign) require human approval first.
- **Repository protection:** The GitHub adapter checks the remote URL before any write operation to prevent accidentally modifying the wrong repository.
