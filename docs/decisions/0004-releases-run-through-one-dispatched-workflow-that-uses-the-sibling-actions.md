---
status: accepted
date: 2026-10-25
decision-makers:
  - maintainers
applies-to:
  - .github/workflows/release.yml
---

# Releases run through one dispatched workflow that uses the sibling actions

## Context and problem statement

The five actions version-bump-action (computes the version), release-preflight-action (checks collisions and drift), manifest-version-update-action (writes the version), ai-changelog-action (writes the changelog) and npm-trusted-publish-action (publishes) exist to be used together, and each repository releases itself with the others. Recorded retroactively from `.github/workflows/release.yml` and the tags.

## Decision

- A maintainer starts the Release workflow by hand with a release type (`prerelease`, `premajor`, `patch`, `minor` or `major`). It has no other trigger.
- It runs the tests, computes the version (version-bump-action), runs the release preflight (the tag is free, the version is not on npm, no drift), writes the version into the manifests (manifest-version-update-action), updates and promotes the changelog (ai-changelog-action, with the `LLM_API_KEY` secret), commits, tags and publishes the GitHub release.
- The action being released is used from the checkout (`uses: ./`) for the step it implements, and the others at their moving major tag (`@v1`).
- Tagging is idempotent: a tag that already exists on the current commit is skipped, one on a different commit fails the run.
- A stable release moves the major and minor tags (`v1`, `v1.1`) to the new release; a prerelease does not. Callers pin the major tag.
- The GitHub release is created with the `RELEASE_TOKEN` secret, not the workflow's own token.

## Consequences

- `CHANGELOG.md` and the manifest version are written by this workflow; nobody edits them by hand.
- A release created with the workflow's own token would not start other workflows; the Concierge's release closer starts from a published release.
- A change to the release workflow changes how a repository releases, so a maintainer reads it.
