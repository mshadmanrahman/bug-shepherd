# Safety Rules: Why Human Gates Exist

This document explains the safety rules in Bug Shepherd and why they exist. Each rule was added because of a real incident or a near-miss. They are not theoretical precautions.

## The Incident

**Date:** March 10, 2026
**What happened:** During a mass triage session, Bug Shepherd's automated system cancelled two bugs (INS-1959 and INS-2311) as "not reproducible." A developer who was actively investigating those bugs flagged both as still live within hours.

**Root cause:** The reproduction checker used Playwright (a headless browser) to test the bugs. One was an Arabic/RTL vertical scrolling issue; the other involved specific viewport behavior. Headless browsers cannot reliably reproduce these types of bugs because:
- They don't render RTL text layout identically to real browsers
- Scroll position and momentum behave differently
- Touch events and viewport-dependent behavior are simulated, not real

**Impact:**
- Developer lost context on bugs they were investigating
- Trust in the triage system was damaged
- Two bugs had to be manually re-opened and re-assigned
- Time wasted on cleanup instead of fixing bugs

**Lesson:** "Agent can't reproduce" does NOT mean "bug doesn't exist." The cost of a false cancellation (lost context, broken trust, wasted developer time) far exceeds the cost of keeping a bug open one more sprint.

## The Rules

### Rule 1: Never Auto-Cancel High False-Negative Categories

**Configured in:** `safety.never_auto_cancel` in `triage.config.yaml`

Certain bug categories have high false-negative rates in automated testing. If a bug's description matches any of these categories, it MUST go to human review, even if the agent couldn't reproduce it.

Default categories:
- **Scroll behavior** — headless browsers simulate scrolling differently
- **Touch interaction** — no real touch events in automation
- **Animation** — timing and rendering differences
- **Viewport-dependent** — responsive behavior varies between headless and real browsers

You should add project-specific categories. Examples:
- "Drag and drop" — Playwright drag simulation is unreliable
- "Hover states" — mobile devices don't have hover
- "Keyboard navigation" — focus management varies
- "Audio/video playback" — media rendering is inconsistent

### Rule 2: Never Auto-Cancel Bugs With Recent Activity

**Configured in:** `safety.recent_activity_days` (default: 90)

If a bug has comments or status updates within the configured window, someone is actively working on it or investigating it. Cancelling such a bug is disruptive and disrespectful of their work.

The system skips these bugs entirely during mass triage.

### Rule 3: Never Auto-Cancel Assigned Bugs

**Configured in:** `safety.skip_assigned` (default: true)

If a bug has an assignee, someone has taken ownership. The triage system should not override that ownership.

### Rule 4: Always Present Results for Human Review

**Enforced in:** `/shepherd-triage` command (Step 7: Human Review Gate)

This is the most important rule. Before ANY tracker write operation (cancel, comment, transition), the complete categorized results must be presented to a human for review.

The system categorizes findings into:
- **Auto-Cancel** — high-confidence, matches auto-cancel rules
- **Needs Human Review** — not reproduced, but doesn't meet auto-cancel criteria
- **Confirmed Reproducible** — bug still exists
- **Cannot Determine** — agent couldn't reach a conclusion

Only after explicit human approval does the system execute tracker updates.

### Rule 5: Bias Toward Keeping Bugs Open

When the agent's confidence is low or the situation is ambiguous, the default action is to keep the bug open and flag it for human review. The reasoning:

| Action | Cost if wrong |
|--------|--------------|
| Keep open (bug is actually fixed) | Bug sits in backlog one more sprint. Minimal cost. |
| Cancel (bug still exists) | Developer loses context. Bug must be re-opened. Trust damaged. High cost. |

The asymmetry is clear: false cancellation is much more expensive than false retention.

### Rule 6: Auto-Cancel Only With Explicit Rules

**Configured in:** `safety.auto_cancel_rules` in `triage.config.yaml`

The only bugs that can be auto-cancelled (without per-bug human review) are those matching explicitly defined rules. For example:

```yaml
auto_cancel_rules:
  - pattern: "deprecated-domain"
    match_field: "description"
    match_regex: "(olddomain\\.com|legacy\\.example\\.com)"
    description: "Bug filed against a domain that no longer exists"
```

This means the user has pre-approved a class of cancellations. The system still reports what it auto-cancelled, but doesn't wait for individual approval.

If `auto_cancel_rules` is empty, ALL non-reproduced bugs go to human review. This is the safest default.

## Adding New Safety Rules

When you encounter a new category of false negatives:

1. Add it to `safety.never_auto_cancel` in your config
2. Document WHY in a comment (future you will thank present you)
3. Run `/shepherd-learn` to capture the lesson

Example:
```yaml
never_auto_cancel:
  - "scroll behavior"      # Playwright scroll simulation differs from real browsers
  - "touch interaction"    # No real touch events in headless mode
  - "animation"            # Timing and rendering differences
  - "print layout"         # Added 2026-04: Playwright print emulation unreliable
```

## The Meta-Lesson

Triage automation is a power tool. Like all power tools, it needs safety guards. The guards might slow you down on the happy path, but they prevent catastrophic failures on the edge cases.

Build trust with your team by never auto-cancelling something they care about. Over time, your triage results will be trusted because your false-positive rate is near zero.
