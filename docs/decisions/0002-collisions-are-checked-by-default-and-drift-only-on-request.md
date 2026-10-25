---
status: accepted
date: 2026-10-25
decision-makers:
  - maintainers
applies-to:
  - action.yml
  - README.md
---

# Collisions are checked by default and drift only on request

## Context and problem statement

A colliding tag or version must always stop a release. The drift check compares against what the caller says is current, so it needs more input. Recorded retroactively from the input defaults in `action.yml` and the README.

## Decision

- `check-remote-tag` and `check-npm-version` default to `true`.
- `check-version-drift` defaults to `false`: it needs `current-version` and `package-name` from the caller. The npm checks need `package-name` too.
- The remote is `origin` unless `git-remote` says otherwise.

## Consequences

- The Release workflows of the sibling repositories turn the drift check on explicitly.
