---
status: accepted
date: 2026-10-25
decision-makers:
  - maintainers
applies-to:
  - scripts/preflight.sh
  - README.md
---

# A stable base is compared only with stable releases

## Context and problem statement

Promoting a prerelease (patch release 4.8.6 after 4.8.6-1) failed the drift check. version-bump-action correctly reports the latest stable tag (4.8.5) as the base for a patch, minor or major bump, but the check compared it with the highest known version including prereleases (4.8.6-1). Fixed in commit a15b144.

## Decision

- A stable `current-version` is compared only with the latest stable npm or tag version.
- A prerelease `current-version` is still compared with everything.
- Genuine drift, a newer stable release than the base, still fails.

## Consequences

- `drift_detected` is relative to the latest known stable version when the current version is stable, as the README says.
