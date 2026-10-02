---
title: "Project Architecture"
description: "How template drift detection, enforcement, and sync automation work."
when_to_read: "When changing drift workflows, sync scripts, or consumer integration."
---

# Template drift detection and sync

- **Detect:** [`shared-workflows` drift scan](https://github.com/26-2-labs/shared-workflows/blob/main/.github/workflows/drift-scan.yml)
  runs weekly (Monday 06:00 UTC) and on `workflow_dispatch`. It discovers
  `{repo, template}` pairs from each repository's `.github/template.yaml`, checks
  out `<workflow-owner>/template-<template>` and `<repo>`, and diffs their
  `.github/` trees while skipping `intentional-drift` paths. It opens or closes
  a `Template drift: <repo> vs <template>` issue. The separate
  [`sync-all-consumers.sh`](https://github.com/26-2-labs/shared-workflows/blob/main/scripts/sync-all-consumers.sh) keeps a
  hardcoded list for bulk syncs.
- **Enforce per PR:** Consumer repositories call the reusable
  [`check-template-drift.yml`](https://github.com/26-2-labs/shared-workflows/blob/main/.github/workflows/check-template-drift.yml)
  with a `template: <name>` input from their own `drift.yml`. CI fails on
  undeclared drift. `templates-repo` defaults to
  `<caller-owner>/template-<name>` and should only be overridden for unusual
  cases.
- **Fix:** [`sync-template.sh`](https://github.com/26-2-labs/shared-workflows/blob/main/scripts/sync-template.sh)
  `<name> 26-2-labs/<repo>` clones `26-2-labs/template-<name>`, copies its
  `.github/` tree into the target while skipping `intentional-drift` paths,
  commits with `-S --signoff`, pushes a `sync/...` branch, and opens a PR.
  The target's `.github/template.yaml` is never overwritten because it carries
  the target's own state.
- **Automate drift PRs:** The shared-workflows drift scan calls the same sync script after
  upserting the tracking issue. Automated commits use `--no-sign` but retain
  DCO sign-off. Local script runs remain GPG-signed.

For example, `sync-template.sh base 26-2-labs/template-python` copies shared
files into the specialized template while respecting its intentional drift.
The drift scan tracks this relationship as `repo: 26-2-labs/template-python`,
`template: base`.
