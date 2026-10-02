---
title: "Project Conventions"
description: "Formatting, validation, Git, and safety conventions for this repository."
when_to_read: "Before making changes, running validation, committing, or opening a PR."
---

## Formatting

- always sort json

## Local validation

Run commands from the repository root. This repository manages `pre-commit`
through `mise.toml`. Use `mise exec --` so commands can find the configured tools
even when the shell has not loaded mise activation.

For a fresh clone, install mise first if needed, then initialize the tools and hooks:

```bash
mise trust mise.toml
mise install
mise exec -- pre-commit install
mise exec -- pre-commit install --hook-type commit-msg
```

Run hooks on the files you changed before committing:

```bash
mise exec -- pre-commit run --files path/to/file.yml path/to/another.md
```

Use `mise exec -- pre-commit run --all-files` when validation of the entire
repository is needed. If hooks modify files, review the changes and rerun the
hooks before staging them.

In shells without mise activation, use `mise exec -- git commit` so installed
Git hooks can also find `pre-commit`. If `pre-commit` is missing from the shell's
PATH, try it through mise before concluding that it is unavailable or bypassing
hooks.

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

- Commit messages are checked by commitizen (local `commit-msg` hook) and commitlint (CI).
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
