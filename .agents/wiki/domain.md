---
title: "Project Domain"
description: "Template and consumer relationships, including specialized templates."
when_to_read: "When working with templates, consumers, or intentional drift."
---

# Templates and consumers

`template-base` is the canonical language-neutral GitHub configuration for
repositories created from it. Its workflows call reusable workflows in the
separate `26-2-labs/shared-workflows` repository.

Each `26-2-labs/template-<name>` repository is a standalone GitHub template.
Its own `.github/` tree is canonical for repositories created from it. Repos
created with "Use this template" inherit that tree, including
`.github/template.yaml`, at creation time.

Each `.github/template.yaml` records `template: <name>` and an
`intentional-drift:` list of paths allowed to differ from the canonical
template. New repositories inherit this file verbatim and edit their local copy
as they diverge intentionally. See [`.github/README.md`](../../.github/README.md)
for the consumer-facing explanation.

## Specialized templates

`template-base` is the root template. Language-specific templates such as
`template-python`, `template-pyloid`, and `template-go` consume `template-base`
for shared files such as `release.yml`, `stale.yml`, and `deps.yml`. They list
language-specific files such as `ci.yml` and `security.yml` under
`intentional-drift`, so a base sync does not overwrite them. The scan in
`shared-workflows` tracks their relationship to this template.

An app repository created from `template-python` compares only with
`26-2-labs/template-python`: its own `template.yaml` says `template: python`.
That specialized template already contains the shared base files and its
Python-specific files.
