# Contributing to Bug Shepherd

Bug Shepherd was born from real-world pain. If you've triaged bugs and learned something the hard way, your contribution is welcome.

## Ways to Contribute

### New Tracker Adapter
Support for another bug tracker (Asana, ClickUp, Shortcut, Azure DevOps, etc.). Use the [New Adapter issue template](https://github.com/mshadmanrahman/bug-shepherd/issues/new?template=new_adapter.md).

An adapter needs:
- Query patterns for listing open bugs
- Fetch patterns for full bug details
- Write patterns for comments, status changes, assignments
- Error handling guidance
- Configuration section for `tracker_config`

Use `adapters/jira.md` as the reference implementation.

### Safety Rules
If you've found a bug category with high false-negative rates in automated testing, share it. These prevent false cancellations.

### Learning Log Patterns
Common lessons that apply across projects. Add to `examples/learning-log-example.md`.

### Reproduction Strategies
Better ways to check if bugs still exist, especially for categories that Playwright struggles with.

### Documentation
Improvements to guides, examples, or architecture docs.

## How to Submit

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature`
3. Make your changes
4. Test the installer if you changed it: `./install.sh /tmp/test-project`
5. Submit a PR with a clear description of what and why

## Style Guide

- Write for PMs, not developers. Plain language over jargon.
- Commands should work without code knowledge.
- Safety rules need a "why" (ideally from a real incident).
- Keep config options minimal. Every option is a decision the user has to make.

## Code of Conduct

Be kind. Bug triage is stressful enough without hostile interactions. We're all trying to make backlogs less painful.
