# Bug Shepherd

**Zero-code bug triage for PMs, designers, and anyone who doesn't live in the codebase.**

Bug Shepherd is a Claude Code skill that turns your bug backlog into a managed operation. It syncs bugs from your tracker, reproduces them on your live site, builds institutional memory from every session, and enforces human approval gates before any destructive action.

Built by a PM who triaged 240+ bugs across 28 locales without writing a single line of product code.

## What It Does

| Command | Purpose | Who Uses It |
|---------|---------|-------------|
| `/shepherd-sync` | Pull open bugs from your tracker into a local backlog | Anyone |
| `/shepherd-start {ID}` | Begin a focused bug investigation session | PM or developer |
| `/shepherd-triage` | Mass-check if old bugs still reproduce (parallel agents) | PM |
| `/shepherd-review` | Quality gate before pushing a fix | Developer |
| `/shepherd-learn` | Capture what you learned this session | Anyone |

## How It Works — Step by Step

Don't worry if you've never used a terminal before. Bug Shepherd guides you through everything. Here's the complete flow, with **you in control at every decision point**:

```
STEP 1: SYNC YOUR BACKLOG
┌─────────────────────────────────────────────────────────┐
│  Your Bug Tracker                                       │
│  (Jira / Linear / GitHub Issues)                        │
│                                                         │
│  240 open bugs, scattered across sprints,               │
│  some from 6 months ago, some from yesterday            │
└────────────────────────┬────────────────────────────────┘
                         │
                    You type:
                  /shepherd-sync
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  Local Backlog (backlog-live.md)                        │
│                                                         │
│  All bugs in one place, sorted by priority then age,    │
│  with status and priority visible at a glance           │
└────────────────────────┬────────────────────────────────┘
                         │
STEP 2: MASS TRIAGE      │
                    You type:
                 /shepherd-triage
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  5 AI Agents Launch in Parallel                         │
│                                                         │
│  Agent 1: Checking bugs 1-6    ───> Live site           │
│  Agent 2: Checking bugs 7-12   ───> Live site           │
│  Agent 3: Checking bugs 13-18  ───> Live site           │
│  Agent 4: Checking bugs 19-24  ───> Live site           │
│  Agent 5: Checking bugs 25-30  ───> Live site           │
│                                                         │
│  Each agent visits the page, follows the steps,         │
│  and takes screenshots as evidence.                     │
└────────────────────────┬────────────────────────────────┘
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  *** HUMAN IN THE LOOP — YOUR DECISION ***              │
│                                                         │
│  Results are shown in 4 categories:                     │
│                                                         │
│  ✅ Auto-Cancel (3)     — Old domain bugs, safe to close│
│  👀 Needs Your Review (18) — Agent unsure, you decide   │
│  🔴 Still Broken (7)    — Confirmed on live site        │
│  ❓ Inconclusive (2)    — Couldn't reach a verdict      │
│                                                         │
│  YOU approve each category. Nothing happens without     │
│  your explicit "go ahead."                              │
└────────────────────────┬────────────────────────────────┘
                         │
STEP 3: FIX A BUG        │  (You or a developer)
                    You type:
              /shepherd-start BUG-1234
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  Session Briefing                                       │
│                                                         │
│  - Full bug details pulled from tracker                 │
│  - Past lessons loaded ("last time this pattern         │
│    needed X, not Y")                                    │
│  - Investigation checklist generated                    │
│  - YOU decide: reproduce, investigate, or delegate      │
└────────────────────────┬────────────────────────────────┘
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  *** HUMAN IN THE LOOP — FIX APPROVAL ***               │
│                                                         │
│  Before any code is written:                            │
│  - Root cause is explained in plain language             │
│  - Proposed fix is described (not just code diff)        │
│  - YOU approve the approach                             │
└────────────────────────┬────────────────────────────────┘
                         │
STEP 4: QUALITY CHECK     │
                    You type:
               /shepherd-review
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  Quality Gate                                           │
│                                                         │
│  - Is the change minimal? (flag if too many lines)      │
│  - Does it only touch what's needed?                    │
│  - Does it follow past lessons?                         │
│  - Linting and type checks pass?                        │
│                                                         │
│  Verdict: PASS / WARN / BLOCK                           │
│  YOU decide whether to ship it                          │
└────────────────────────┬────────────────────────────────┘
                         │
STEP 5: LEARN             │
                    You type:
               /shepherd-learn
                         │
                         v
┌─────────────────────────────────────────────────────────┐
│  Institutional Memory                                   │
│                                                         │
│  Lesson captured: "This type of bug happens because..." │
│  Next time, the system loads this lesson automatically. │
│  Your team gets smarter with every session.             │
└─────────────────────────────────────────────────────────┘
```

### The Golden Rule: You're Always in Control

Every step that modifies your bug tracker requires **your explicit approval**. Bug Shepherd automates the tedious work (syncing, checking, categorizing) but never makes decisions for you. This isn't just a philosophy; it's a safety rule [born from a real incident](docs/SAFETY-RULES.md) where automated cancellation went wrong.

## Why This Exists

Most bug triage tools assume you can read the code. Bug Shepherd assumes you can't, and that's the point.

As a PM, you understand the product better than anyone. You know which bugs matter, which ones users complain about, and which ones are stale. But you shouldn't need to read source code to manage a bug backlog.

Bug Shepherd gives you:
- **Automated reproduction checks** without writing test code
- **Institutional memory** that prevents the same mistakes twice
- **Safety gates** learned from real incidents (we once falsely cancelled bugs a developer was actively fixing)
- **Parallel processing** that triages 30 bugs in 5 minutes

## Quick Start

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and configured
- A bug tracker: Jira (via Atlassian MCP), Linear, or GitHub Issues
- [Playwright MCP](https://github.com/anthropics/claude-code/blob/main/docs/mcp.md) for reproduction checks (optional but recommended)
- A live/staging URL where bugs can be reproduced

### Installation

```bash
# Clone into your project
git clone https://github.com/mshadmanrahman/bug-shepherd.git

# Run the installer
cd bug-shepherd
chmod +x install.sh
./install.sh
```

The installer will:
1. Ask which bug tracker you use
2. Ask for your project key and live URL
3. Generate a `triage.config.yaml` tailored to your project
4. Copy commands to your `.claude/commands/` directory
5. Create template files for your learning log and backlog

### Manual Installation

1. Copy `commands/` to your project's `.claude/commands/`
2. Copy `agents/` to your project's `.claude/agents/`
3. Copy `templates/` contents to your project's `.claude/`
4. Copy `triage.config.yaml` to your project's `.claude/`
5. Edit `triage.config.yaml` with your project details

## Configuration

All project-specific settings live in `.claude/triage.config.yaml`:

```yaml
project:
  name: "My Project"
  tracker: jira                    # jira | linear | github-issues
  tracker_project: "PROJ"         # Your project key
  live_url: "https://example.com" # Where to reproduce bugs

reproduction:
  tool: playwright                 # playwright | manual
  viewports: [1440, 820, 375]     # Screen widths to test

safety:
  never_auto_cancel:               # Bug categories with high false-negative rates
    - "scroll behavior"
    - "touch interaction"
    - "animation"
  auto_cancel_rules:
    - pattern: "deprecated-domain"
      description: "Bug filed against a domain that no longer exists"
  recent_activity_days: 90         # Skip bugs with recent comments

team:
  git_email: ""                    # Required git email for commits
  assignee_id: ""                  # Your tracker user ID
```

See `examples/triage.config.example.yaml` for a fully commented configuration.

## The Safety Story

On March 10, 2026, our triage system falsely cancelled two bugs that a developer was actively investigating. The developer caught it immediately, but the damage was done: lost context, broken trust, and a lesson learned the hard way.

**What we changed:**
- Automated cancellation is now restricted to high-confidence cases only (e.g., bugs against domains that no longer exist)
- All other "not reproduced" findings go to a **HUMAN REVIEW** queue
- Certain bug categories (scroll, touch, viewport, animation) are **never** auto-cancelled because headless browsers can't reliably reproduce them
- Bugs with recent activity or an active assignee are always flagged for human review

Read the full story in [docs/SAFETY-RULES.md](docs/SAFETY-RULES.md).

## Supported Trackers

| Tracker | MCP Required | Status |
|---------|-------------|--------|
| Jira | [Atlassian MCP](https://modelcontextprotocol.io/integrations/atlassian) | Full support |
| Linear | Linear MCP | Adapter included |
| GitHub Issues | Built-in `gh` CLI | Adapter included |

## Project Structure

```
bug-shepherd/
  README.md                  # You are here
  LICENSE                    # MIT
  install.sh                 # Interactive installer
  triage.config.yaml         # Default configuration template
  commands/                  # Claude Code slash commands
    shepherd-sync.md         # Backlog synchronization
    shepherd-start.md        # Bug fix session setup
    shepherd-triage.md       # Mass reproducibility check
    shepherd-review.md       # Quality gate
    shepherd-learn.md        # Capture lessons
  agents/
    reproduction-checker.md  # Parallel reproduction agent
  templates/
    learning-log.md          # Institutional memory template
    quality-gate.md          # Pre-push checklist
    backlog-live.md          # Synced bug inventory
    triage-log.md            # Triage session results
  adapters/
    jira.md                  # Jira-specific patterns
    linear.md                # Linear-specific patterns
    github-issues.md         # GitHub Issues patterns
  docs/
    ARCHITECTURE.md          # System design and workflows
    SAFETY-RULES.md          # Why human gates exist
    CUSTOMIZATION.md         # Adding project-specific rules
  examples/
    triage.config.example.yaml
    learning-log-example.md
```

## Contributing

Bug Shepherd was born from real-world pain. If you've triaged bugs and learned something the hard way, contributions are welcome:

- **New adapter**: Support for another bug tracker
- **Safety rules**: Categories that shouldn't be auto-cancelled
- **Learning log patterns**: Common lessons worth sharing
- **Reproduction strategies**: Better ways to check if bugs still exist

## License

MIT. See [LICENSE](LICENSE).

---

*Built with [Claude Code](https://claude.com/claude-code). Herding bugs so you don't have to read the code.*
