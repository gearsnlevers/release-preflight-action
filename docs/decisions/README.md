# Decision records

A decision record explains one lasting decision: why we chose it, what it means and what we gave up. Records are plain Markdown in the repository, so every contributor and every coding agent, whatever tool it uses, reads the same thing.

This folder holds the decisions that concern **release-preflight-action**: what the action does and does not do, how it behaves, and how it is released. It is one of five small actions that release each other (version-bump-action, release-preflight-action, manifest-version-update-action, ai-changelog-action and npm-trusted-publish-action). Decisions about the Repo Concierge that runs this repository's gates are recorded in the [repo-concierge](https://github.com/vscheuber/repo-concierge/tree/main/docs/decisions) repository.

## Rules

- An accepted record is binding for humans and agents until a newer record supersedes it. An agent whose task conflicts with one stops and says so in the issue or pull request.
- Only lasting decisions get a record: something you would be surprised to see reversed without discussion. Not minor implementation notes or routine refactors.
- An accepted record is not edited, except for typos and for a `Known deviations` section, a status note that may be added, edited or removed, without a new record, in the pull request that fixes the deviation or opens its tracking issue. A changed decision is a new record: set the old record's status to `superseded by NNNN` and update the index.
- A pull request that changes a decided behaviour adds or supersedes a record in the same pull request.
- Records are public. No credentials, tenant names, security-incident details or personal information.
- `applies-to:` lists path globs where that helps; `issues:` lists related issues and pull requests as `owner/repo#N`.
- A record written after the fact says so in its context ("Recorded retroactively from ...") and describes what the code and the history show.

## Writing one

Copy `template.md` to `NNNN-short-title.md` (the next number, starting at 0001), fill it in (aim for under 40 lines) and add a row to the index. Besides the three required sections, a record may have `## Known deviations` and `## Alternatives considered`. `node .github/scripts/validate-decision-records.mjs docs/decisions` checks this folder; CI runs the same check.

## Index

| # | Title | Status |
| - | - | - |
| 0001 | [The action validates a candidate release and changes nothing](0001-the-action-validates-a-candidate-release-and-changes-nothing.md) | accepted |
| 0002 | [Collisions are checked by default and drift only on request](0002-collisions-are-checked-by-default-and-drift-only-on-request.md) | accepted |
| 0003 | [A stable base is compared only with stable releases](0003-a-stable-base-is-compared-only-with-stable-releases.md) | accepted |
| 0004 | [Releases run through one dispatched workflow that uses the sibling actions](0004-releases-run-through-one-dispatched-workflow-that-uses-the-sibling-actions.md) | accepted |
