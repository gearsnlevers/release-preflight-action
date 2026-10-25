---
status: accepted
date: 2026-10-25
decision-makers:
  - maintainers
applies-to:
  - action.yml
  - scripts/preflight.sh
  - README.md
---

# The action validates a candidate release and changes nothing

## Context and problem statement

Collisions and drift are cheaper to catch before anything is tagged or published than after. Recorded retroactively from the README and `.github/agentic-workflows/scope.md`.

## Decision

- The action checks that the candidate tag does not exist in the remote, that the candidate version is not on npm, and, when asked, that the current version is not behind the latest known version. It outputs `passed`, `tag_exists`, `npm_version_exists`, `drift_detected` and the latest versions it found, and fails fast on a collision.
- It does not compute versions, update manifests, publish or create tags. Those are other actions: version-bump-action (computes the version), release-preflight-action (checks collisions and drift), manifest-version-update-action (writes the version), ai-changelog-action (writes the changelog) and npm-trusted-publish-action (publishes).
- It checks git and npm only. Other registries (PyPI, crates.io, Maven) or other version control need a maintainer's decision.

## Consequences

- Git tag checks need full history and tags, so the release workflow fetches them.
