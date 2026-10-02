---
title: "Project Requirements"
description: "Private template access and token requirements for drift checks and syncs."
when_to_read: "When changing template drift checks, sync automation, or token permissions."
---

# Private template access

`template-<name>` repositories are private. A cross-repository
`actions/checkout` in `check-template-drift.yml` uses the caller's own
`GITHUB_TOKEN`, which cannot read a different private repository. The
`shared-workflows` reusable-workflow `access_level` is already `organization`;
that setting governs `uses:` calls, not `actions/checkout` file access.

- When a template repository checks itself (its `template.yaml` names that
  same `template-<name>`), `check-template-drift.yml` skips the comparison.
  Self-comparison cannot reveal drift and needs no additional token. This is
  currently the case for `template-base`.
- Any cross-repository check requires `TEMPLATE_REPO_TOKEN` in **every**
  repository that runs it. The token needs `Contents: read` on the relevant
  private repositories. This includes consumer checks against a template and
  the `shared-workflows` drift scan checks against templates and consumers.
  A consumer's `drift.yml` uses `secrets: inherit` to forward the token to the
  reusable workflow when present.
- Automated drift PRs require a separate `TEMPLATE_SYNC_TOKEN` with template
  read access and consumer contents and pull-request write access. The scan's
  `TEMPLATE_REPO_TOKEN` remains read-only.
