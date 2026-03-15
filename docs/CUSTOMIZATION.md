# Customization Guide

Bug Shepherd is designed to be adapted to your project. This guide explains what to customize and how.

## Quick Customization Checklist

After installation, customize these in order:

1. [ ] `triage.config.yaml` — project name, tracker, live URL
2. [ ] `safety.never_auto_cancel` — add project-specific false-negative categories
3. [ ] `safety.auto_cancel_rules` — define high-confidence cancellation patterns (optional)
4. [ ] `review.tech_stack_rules` — add project-specific code review checks (optional)
5. [ ] `team.*` — git email, tracker user ID

## Adding Project-Specific Safety Categories

Your project likely has bug categories that headless browsers can't reliably test. Add them to `safety.never_auto_cancel`:

```yaml
safety:
  never_auto_cancel:
    # Default categories (keep these)
    - "scroll behavior"
    - "touch interaction"
    - "animation"
    - "viewport-dependent"

    # Examples of project-specific additions:
    - "WebGL rendering"       # If your app uses WebGL
    - "PDF generation"        # If bugs involve generated PDFs
    - "email templates"       # Can't test email rendering in browser
    - "dark mode"             # If dark mode has nuanced CSS
    - "accessibility"         # Screen reader behavior can't be automated
    - "third-party widget"    # Embedded widgets (chat, analytics) behave differently
```

## Adding Auto-Cancel Rules

If your project has a clear pattern of non-reproducible bugs (e.g., bugs against a deprecated version), define auto-cancel rules:

```yaml
safety:
  auto_cancel_rules:
    # Example: bugs against old API version
    - pattern: "v1-api"
      match_field: "description"
      match_regex: "(api/v1|v1\\.example\\.com)"
      description: "Bug filed against deprecated v1 API"

    # Example: bugs against removed feature
    - pattern: "legacy-dashboard"
      match_field: "summary"
      match_regex: "(old dashboard|legacy dashboard|classic view)"
      description: "Bug filed against removed legacy dashboard"

    # Example: bugs against test/staging environment
    - pattern: "staging-only"
      match_field: "description"
      match_regex: "(staging\\.example\\.com|test\\.example\\.com)"
      description: "Bug filed against staging environment only"
```

**Be conservative.** Auto-cancel rules bypass individual human review. Only add rules for patterns you're very confident about.

## Adding Tech Stack Review Rules

If you're using `/shepherd-review` for code quality, add project-specific checks:

```yaml
review:
  tech_stack_rules:
    # Prevent common mistakes
    - name: "No console.log in production"
      check: "grep -rn 'console.log' src/ --include='*.ts' --exclude='*.test.ts' --exclude='*.spec.ts'"
      severity: "warn"
      message: "Remove console.log statements before merging"

    - name: "No TODO comments in new code"
      check: "git diff main...HEAD | grep '+.*TODO'"
      severity: "warn"
      message: "Resolve TODO comments before merging"

    # Framework-specific rules
    - name: "No inline styles in React"
      check: "git diff main...HEAD -- '*.tsx' | grep '+.*style={{'"
      severity: "warn"
      message: "Use CSS classes or styled-components instead of inline styles"

    # Security rules
    - name: "No hardcoded URLs"
      check: "git diff main...HEAD | grep -E '\\+.*(https?://[a-z]+\\.(com|io|org))' | grep -v test | grep -v spec"
      severity: "warn"
      message: "Use environment variables for URLs"

    # Blocking rules (prevent push)
    - name: "No secrets in code"
      check: "git diff main...HEAD | grep -iE '\\+.*(api_key|secret|password|token)\\s*[:=]\\s*[\"'\\']'"
      severity: "block"
      message: "SECURITY: Possible secret in code. Use environment variables."
```

## Multi-Locale Sites

If your site supports multiple locales:

```yaml
project:
  live_url: "https://example.com/{locale}"
  locales:
    - "en"
    - "de"
    - "fr"
    - "es"
    - "ar"    # RTL locale
    - "ja"    # CJK locale
```

The reproduction checker will test bugs at the locale specified in the bug report. If no locale is specified, it tests the first locale in the list.

**Tip:** Add RTL locales (Arabic, Hebrew, Farsi) to `never_auto_cancel` if your site supports them. RTL layout bugs are notoriously hard to reproduce in headless browsers.

## Custom Reproduction Strategies

The default reproduction strategy uses Playwright to navigate and screenshot. For projects with specific needs, you can customize:

### Authentication Required
If your site requires login to reproduce most bugs:

```yaml
reproduction:
  tool: "playwright"
  # Add authentication steps
  auth:
    url: "https://example.com/login"
    username_field: "#email"
    password_field: "#password"
    submit_button: "#login-btn"
    # Store credentials in environment variables, NOT in config
    username_env: "BUG_SHEPHERD_USERNAME"
    password_env: "BUG_SHEPHERD_PASSWORD"
```

### Manual Mode
If Playwright isn't suitable (native app, complex SPA, etc.):

```yaml
reproduction:
  tool: "manual"
```

In manual mode, `/shepherd-triage` skips automated reproduction and instead:
1. Organizes the backlog by age and category
2. Generates a structured review list
3. You check each bug manually and report results
4. The system handles tracker updates based on your findings

## Extending the Learning Log

The learning log is your project's institutional memory. To get the most value:

1. **Be specific:** "Button overflow in German locale" is better than "UI bug"
2. **Capture the wrong assumption:** "I assumed the bug was in CSS, but it was a missing translation key"
3. **Note patterns:** "Third time a long-text overflow in a flex container was caused by missing max-width"
4. **Suggest prevention:** "We should add a linting rule for flex containers without overflow handling"

Over time, your learning log becomes a troubleshooting guide specific to your project.

## Team Collaboration

Bug Shepherd's state files (backlog, learning log, triage logs) can be committed to your repository. This means:

- Multiple team members share the same institutional memory
- Triage logs provide an audit trail
- Learning log entries from one person help everyone
- Backlog is always in sync across the team

**Recommended `.gitignore` additions:**
```
# Don't ignore these (they're valuable shared state)
# .claude/backlog-live.md
# .claude/learning-log.md
# .claude/triage-log-*.md

# Do ignore these (personal/temporary)
.claude/triage.config.yaml   # Contains personal tracker IDs
```

**Recommended approach:** Commit `triage.config.yaml` with team-shared values, and use environment variables or a `.claude/triage.config.local.yaml` override for personal settings.
