# Project scope

Read by the issue gate's scope check (Repo Concierge). The check is off for a repository without this file.

## Purpose

Validate a candidate release version and tag against git and npm before publishing, failing fast on collisions or drift.

## In scope

- Checking that the candidate tag does not already exist in the remote
- Checking that the candidate version is not already published on npm
- The version drift check against the latest known npm or tag version
- The action's inputs and outputs, documentation, tests, and release pipeline

## Out of scope

<!-- Every item here can close an issue automatically as not planned. Keep the list short and specific. -->

- Computing versions, updating manifests, publishing to npm or creating tags. The action only validates; it does none of these (stated in the README).

## Needs a maintainer

- Preflight checks for registries other than npm, such as PyPI, crates.io or Maven
- Checks against version control other than git

## Where to go instead

- Computing versions: version-bump-action. Updating manifests: manifest-version-update-action. Publishing: npm-trusted-publish-action.
