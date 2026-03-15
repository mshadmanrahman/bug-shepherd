# LinkedIn Post Draft — Bug Shepherd Launch

## Context
- Yesterday: shared PM Pilot (the PM toolkit for Claude Code)
- Today: follow-up with Bug Shepherd (the zero-code triage system)
- Audience: PMs, designers, QA, product people who are curious about AI tools but intimidated by terminals
- Key hooks: human-in-the-loop, zero code knowledge, open source, real incident story

---

## Option A: The Sequel Hook

Yesterday I shared my PM toolkit for Claude Code.

Today I'm open-sourcing the part people asked about most: how I triaged 240+ bugs without reading a single line of code.

Meet Bug Shepherd.

It's a free, open-source triage system built for people who don't live in the codebase. PMs. Designers. QA. Anyone with a Jira backlog and a desire to actually manage it.

Here's what it does:

1. Syncs your entire bug backlog locally (Jira, Linear, or GitHub Issues)
2. Launches parallel AI agents that visit your live site and check if old bugs still exist
3. Categorizes findings and presents them to YOU for review
4. You approve what gets closed. Nothing happens without your say.
5. Captures lessons from every session so the system gets smarter over time

The human-in-the-loop part isn't a buzzword here. It's a safety rule.

We learned this the hard way: our AI triage once auto-cancelled two bugs a developer was actively investigating. The developer caught it. Trust was damaged. We rebuilt the entire system around one principle:

AI does the tedious work. Humans make the judgment calls.

Now every cancellation requires your explicit approval. Certain bug types (scroll, touch, animation) are NEVER auto-cancelled because headless browsers can't reliably reproduce them.

The result? 30 bugs triaged in 5 minutes. Zero false cancellations since the redesign.

No terminal experience needed. The installer asks 4 questions and sets everything up.

Link in comments.

#ProductManagement #BugTriage #AI #OpenSource #ClaudeCode #HumanInTheLoop

---

## Option B: The Pain-First Hook

Every PM I know has a backlog problem.

Not a "we have bugs" problem. A "we have 240 bugs and no one knows which ones still matter" problem.

You can't read the code. You can't reproduce half of them. And manually checking each one takes forever.

So I built Bug Shepherd.

It's a free tool that triages your bug backlog without requiring any code knowledge:

- Pulls your full backlog from Jira, Linear, or GitHub Issues
- Sends AI agents to your live site to check if bugs still exist
- Groups results: confirmed, can't reproduce, needs your judgment
- YOU decide what to close. Human approval required at every step.

The "human-in-the-loop" isn't marketing. It's a hard rule.

Story time: we once auto-cancelled bugs a developer was actively fixing. They caught it immediately. Trust was broken. We rebuilt with one principle: AI handles the grunt work, humans make every decision that affects someone else's work.

Since then: 240+ bugs triaged. Zero false cancellations.

Yesterday I shared PM Pilot, my PM toolkit for Claude Code. Bug Shepherd is the part of my workflow that people kept asking about.

It's open source. It's free. It takes 5 minutes to set up.

Link in comments.

#ProductManagement #BugTriage #OpenSource #AI #ClaudeCode

---

## Option C: The Provocative Hook

A PM triaged 240 bugs in a week.

Without reading a single line of code.

Without asking engineering to "prioritize the backlog."

Without a QA team.

Here's how.

I built Bug Shepherd: an AI-powered triage system that does the tedious part (checking if old bugs still reproduce) while keeping you in control of every decision.

The flow:
Step 1: Sync your Jira/Linear/GitHub backlog (one command)
Step 2: AI agents visit your live site and check each bug (5 agents in parallel)
Step 3: Results land in YOUR inbox, categorized:
  - Safe to close (old domain, feature removed)
  - Still broken (confirmed on live site)
  - Needs your judgment (agent unsure)
Step 4: You approve. Nothing closes without your say.
Step 5: Lessons are captured. Next time, the system is smarter.

Human-in-the-loop at EVERY decision point. Not as a checkbox. As a safety guarantee.

(We learned this the hard way. Once, the AI cancelled bugs a developer was actively fixing. Never again.)

This is a follow-up to PM Pilot, the PM toolkit I shared yesterday. Bug Shepherd is the specific workflow people kept asking about.

It's free. It's open source. Zero terminal experience needed.

Link in comments.

#ProductManagement #AI #OpenSource #BugTriage #HumanInTheLoop #ClaudeCode

---

## Recommended: Option A

Option A works best as a sequel because:
- Opens by referencing yesterday's post (continuity for followers)
- "The part people asked about most" creates social proof
- Numbered steps make it scannable
- The incident story is personal and credible
- Ends with the low-barrier CTA ("4 questions and you're set up")

---

## DALL-E Image Prompts

### Prompt 1: The Shepherd Metaphor (Recommended)
```
A warm, friendly illustration of a shepherd (wearing a modern hoodie and glasses,
not traditional clothing) gently guiding a flock of small, colorful bug icons
(ladybugs, beetles) through a gate. The gate has a glowing "HUMAN REVIEW" sign.
Some bugs go through the gate (approved), some are redirected to a "STILL OPEN"
pen. The shepherd holds a staff that looks like a modern cursor/pointer. Soft
gradient background in calming blues and greens. Flat illustration style,
tech-friendly, approachable, NOT intimidating. No code or terminals visible.
```

### Prompt 2: The Control Panel
```
A clean, modern illustration of a person (PM, business casual) sitting at a
simple desk with a single large screen. The screen shows a friendly dashboard
with bug cards being sorted into three columns: "Fixed" (green), "Your Call"
(amber), "Still Open" (red). Small AI robot assistants are carrying bug cards
to the columns, but the person has their hand on a large friendly "APPROVE"
button. The vibe is calm and in-control, not chaotic. Flat vector style,
warm colors, no code visible anywhere. The person looks confident, not
overwhelmed.
```

### Prompt 3: The Before/After
```
Split illustration, left side vs right side. LEFT: A stressed PM buried under
a mountain of colorful bug tickets, papers flying, chaotic, overwhelmed,
red/orange tones. RIGHT: The same PM calmly reviewing a neat organized list
on a tablet, small AI shepherd dog sitting beside them, bug tickets neatly
sorted into labeled bins, peaceful, green/blue tones. Clean flat illustration
style, warm and approachable, no code or terminal visible.
```

### Recommended: Prompt 1 (The Shepherd Metaphor)
Directly maps to the product name, memorable, shareable, and the "HUMAN REVIEW"
gate sign subtly communicates the human-in-the-loop message without saying it.
