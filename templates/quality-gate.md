# Quality Gate Checklist

Use this checklist before pushing any bug fix. Run `/shepherd-review` for automated checks.

## Pre-Investigation

- [ ] Confirmed the bug is in THIS project's codebase (not a third-party service, different repo, or CMS)
- [ ] Read the FULL bug report including comments and attachments (titles can be misleading)
- [ ] Checked the learning log for related lessons

## Investigation

- [ ] Reproduced the bug on the live/staging site
- [ ] Tested at all configured viewports
- [ ] Tested with the exact URL from the bug report
- [ ] Identified the root cause (not just a symptom)

## Implementation (if fixing)

- [ ] Proposed the fix before writing code (got approval)
- [ ] Change is minimal (under the configured line threshold)
- [ ] No unrelated changes included
- [ ] No new dependencies added for a bug fix
- [ ] Every changed line directly addresses the bug

## Pre-Push

- [ ] Ran linting and type checks on changed files
- [ ] Tested the fix at all configured viewports
- [ ] Tested adjacent features for regressions
- [ ] Re-read the diff line by line
- [ ] Git commit email matches team configuration

## PR

- [ ] Title references the ticket ID
- [ ] Description explains root cause, not just what was changed
- [ ] Before/after evidence included (if configured)
- [ ] Bug tracker ticket linked

## Red Flags (Stop and Reconsider)

If any of these are true, your approach may be wrong:

- "Let me refactor this while I'm here"
- "This component should use a different pattern"
- "I'll make this configurable for future use"
- "While we're here, let's also fix..."
- Changing a file not mentioned in the bug report
- Adding a new dependency
- The fix is longer than the bug description
