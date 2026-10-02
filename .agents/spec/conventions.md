---
title: "Project Conventions"
description: "Formatting, validation, Git, and safety conventions for this repository."
when_to_read: "Before making changes, running validation, committing, or opening a PR."
---

## Formatting

- always sort json

## Local validation

This repository has no `mise.toml`, `.pre-commit-config.yaml`, or CI
workflows, so nothing validates changes automatically. Review changes by hand
before committing:

- Check that Markdown renders correctly, including links and tables.
- Keep JSON keys sorted.

If a `pre-commit` Git hook is installed from another setup, it skips itself
because there is no `.pre-commit-config.yaml`. When the hook cannot find
`pre-commit` at all, run the commit as `mise exec pre-commit@latest -- git commit`.

## Git

Use Conventional Commits for every commit:

```text
<type>(<optional scope>): <description>
```

### Allowed types

- `build` - build system or tooling changes
- `chore` - maintenance, dependency updates, or config
- `ci` - CI/CD workflow changes
- `docs` - documentation-only changes
- `feat` - user-facing site feature or content addition
- `fix` - bug fix
- `perf` - code changes that improve performance
- `refactor` - code changes that neither fix a bug nor add a feature
- `style` - formatting or visual-only code style changes
- `test` - adding or fixing tests

### Guidelines

- No hook or CI checks commit messages here, so follow this format by hand.
- Commit body lines must not exceed 100 characters.
- Do not amend or force-push commits that have already been published.
- Do not add `Co-authored-by` trailers or authorship/attribution statements to
  commits or pull requests, including AI assistant credit in titles or bodies.
  Keep required DCO `Signed-off-by` trailers.
- Keep commits atomic: one logical change per commit.
- Keep the subject under 72 characters.
- Mark breaking changes with `!` before the colon and explain the impact in the commit body.
- Use the commit body to explain why a change was made when the subject is not enough.
- Write the subject in imperative mood: "add" instead of "added".

## Boundaries & Safety

- Do not delete existing documentation files.
- Never rotate API keys without notifying the security channel.
