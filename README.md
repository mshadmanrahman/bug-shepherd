# Bug Shepherd

<p align="center">
  <img src="assets/hero.png" alt="Bug Shepherd" width="720" />
</p>

You have a bug backlog. It's been sitting there for weeks. Some of those bugs might already be fixed. Some are critical and no one knows it. Most of them are just... there, quietly making you feel behind.

Bug Shepherd is a Claude Code skill that turns that backlog into a managed operation. It syncs your bugs, checks which ones still exist on your live site, surfaces the ones that matter, and remembers what you learn — session after session.

No code required. No engineering degree needed. Just you, your backlog, and a tool that does the tedious work so you can make the decisions.

Built by a PM who triaged 240+ bugs across 28 locales without writing a single line of product code.

---

New to Claude Code? Start at [claudecodeguide.dev](https://claudecodeguide.dev) first. It takes about 20 minutes and makes everything here much easier.

---

## What It Does

Each command below is a Claude Code slash command — you type it into Claude Code's chat and it runs. Here's what each one does and when to use it:

| Command | What it does | When to use it |
|---------|-------------|----------------|
| `/shepherd-sync` | Pulls all open bugs from your tracker into a single local list, sorted by priority and age | Start here. Do this at the beginning of every triage session. |
| `/shepherd-triage` | Sends AI agents to check 30 bugs at once against your live site — with screenshots as evidence | Use this when you want to know which bugs still exist before deciding what to fix |
| `/shepherd-start {ID}` | Opens a focused investigation session for one specific bug, loaded with history and context | Use this when you're ready to dig into a bug — or hand it to a developer with full context |
| `/shepherd-review` | Runs a quality check before any fix is shipped — flags risky changes, checks patterns | Use this before pushing any fix. It's your last line of defence. |
| `/shepherd-learn` | Captures what you learned from a session so the system gets smarter next time | Use this at the end of every session. Takes 30 seconds, saves hours later. |

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

Every step that modifies your bug tracker requires **your explicit approval**. Bug Shepherd automates the tedious work — syncing, checking, categorising — but never makes decisions for you. This isn't just a philosophy; it's a safety rule [born from a real incident](docs/SAFETY-RULES.md) where automated cancellation went wrong.

## Your First Session

Here's what the first 30 minutes looks like from zero.

**Minutes 0–5: Install and configure**
Run the installer (see below). It asks three questions: which tracker you use, your project key, and your live site URL. That's it.

**Minutes 5–10: Sync your backlog**
Type `/shepherd-sync` in Claude Code. Watch your bugs pull in from Jira, Linear, or GitHub Issues into a single sorted file. You'll see how many you actually have, and what's oldest.

**Minutes 10–20: Run your first triage**
Type `/shepherd-triage`. Five agents will fan out across your backlog and check bugs against your live site. Get a coffee. Come back to a categorised results view.

**Minutes 20–30: Review the results**
Bug Shepherd shows you four buckets: safe to close, needs your review, confirmed broken, and inconclusive. You go through each category and approve or push back. Nothing gets closed without you saying so.

By the end of that 30 minutes, you'll know the real state of your backlog — not what the ticket statuses say, but what's actually broken on your site right now.

## Why This Exists

Most bug triage tools assume you can read the code. Bug Shepherd assumes you can't, and that's the point.

As a PM, you understand the product better than anyone. You know which bugs matter, which ones users complain about, and which ones have been sitting there long enough to be stale. But you shouldn't need to read source code to manage a bug backlog.

Bug Shepherd gives you:
- **Automated reproduction checks** without writing test code
- **Institutional memory** that prevents the same mistakes twice
- **Safety gates** learned from real incidents (we once falsely cancelled bugs a developer was actively fixing)
- **Parallel processing** that triages 30 bugs in 5 minutes

## Installation

### What you need before you start

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed. Never done this? Go to [claudecodeguide.dev](https://claudecodeguide.dev) first — it walks you through setup in plain English.
- A bug tracker: Jira (via Atlassian MCP), Linear, or GitHub Issues
- A live or staging URL where bugs can be reproduced
- [Playwright MCP](https://github.com/anthropics/claude-code/blob/main/docs/mcp.md) — optional, but recommended if you want automated screenshots

### Run the installer

```bash
# Clone the repository
git clone https://github.com/mshadmanrahman/bug-shepherd.git

# Run the installer
cd bug-shepherd
chmod +x install.sh
./install.sh
```

The installer will ask you three questions and set everything up:

1. Which bug tracker do you use? (Jira / Linear / GitHub Issues)
2. What is your project key? (e.g. `PROJ` in Jira, or your repo name)
3. What is your live site URL? (e.g. `https://app.yourproduct.com`)

It then generates a config file tailored to your project and copies all the commands to your Claude Code setup.

### Manual installation (if you prefer)

1. Copy `commands/` to your project's `.claude/commands/`
2. Copy `agents/` to your project's `.claude/agents/`
3. Copy `templates/` contents to your project's `.claude/`
4. Copy `triage.config.yaml` to your project's `.claude/`
5. Edit `triage.config.yaml` with your project details

## Configuration

All project-specific settings live in `.claude/triage.config.yaml`. The installer generates this for you, but here's what it looks like:

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

See `examples/triage.config.example.yaml` for a fully commented version.

## The Safety Story

On March 10, 2026, the triage system falsely cancelled two bugs that a developer was actively investigating. The developer caught it immediately, but the damage was done: lost context, broken trust, and a lesson learned the hard way.

**What changed after that:**
- Automated cancellation is now restricted to high-confidence cases only (for example, bugs filed against domains that no longer exist)
- All other "not reproduced" findings go to a human review queue — you decide
- Certain bug categories (scroll, touch, viewport, animation) are never auto-cancelled, because headless browsers can't reliably reproduce them
- Bugs with recent activity or an active assignee are always flagged for human review

Read the full story in [docs/SAFETY-RULES.md](docs/SAFETY-RULES.md).

## Supported Trackers

| Tracker | Setup required | Status |
|---------|---------------|--------|
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

## Part of the PM Toolkit Family

Bug Shepherd is one tool in a set built for PMs using Claude Code. Each one is independent — install what you need.

| Tool | What it does |
|------|-------------|
| [pm-pilot](https://github.com/mshadmanrahman/pm-pilot) | Claude Code configured for PMs. Meeting prep, PRDs, market sizing — 25 skills, ready to install. |
| [tech-to-pm-translator](https://github.com/mshadmanrahman/tech-to-pm-translator) | Turns developer docs into PM language. Structured knowledge bases, not summaries. |
| [morning-digest](https://github.com/mshadmanrahman/morning-digest) | Your morning briefed in 30 seconds. Calendar, email, Slack, and action items in one digest. |
| [claudecode-guide](https://github.com/mshadmanrahman/claudecode-guide) | The friendly guide to Claude Code. Zero jargon — from first install to daily operating system. |
| [root-kg](https://github.com/mshadmanrahman/root-kg) | Your knowledge graph. Ask questions across all your notes, meetings, and emails — cited answers in plain English. |
| **bug-shepherd** | **You are here** |

Start at [claudecodeguide.dev](https://claudecodeguide.dev) if you're new to Claude Code.

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
