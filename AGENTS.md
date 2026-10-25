# release-preflight-action: instructions for coding agents

Validates a candidate release version and tag against git and npm before publishing, and nothing else. It is a composite GitHub Action that release workflows use at its moving major tag (`vscheuber/release-preflight-action@v1`). It is one of five small actions that release each other: version-bump-action, release-preflight-action, manifest-version-update-action, ai-changelog-action and npm-trusted-publish-action; each does one job and points to the others for the rest. Read [README.md](README.md) and `.github/agentic-workflows/scope.md` (what is in and out of scope) first.

## Layout

- `action.yml`: the interface (inputs, outputs, steps). `scripts/preflight.sh`: the implementation, which needs `bash`, `npm`, `node` and `git`. `test/preflight.test.sh`: its tests.
- `README.md`: the documented inputs and outputs, an example and the prerequisites.
- `.github/workflows/`: `ci.yml`, `release.yml` and `decision-records.yml` are this repository's own. The rest belongs to the Repo Concierge (below).
- `CHANGELOG.md` is written by the Release workflow.

## Checks

- `npm test` runs `bash test/preflight.test.sh`: the real script in a temporary workspace. CI runs it.
- `node .github/scripts/validate-decision-records.mjs docs/decisions` checks the decision records.

## Rules

- **Inputs and outputs are a public interface.** Callers pin the major tag. Do not remove or rename an input or output, or change a default, in a way that changes what an existing caller gets. Add new ones with defaults that keep today's behaviour, and keep `action.yml` and the README in step.
- **Add or change a test in `test/` for every behaviour change.** The tests run the real script.
- **Stay within the action's job.** What is out of scope is listed in `.github/agentic-workflows/scope.md` with the sibling action to use instead; a change beyond it is a decision for a maintainer.
- **Do not edit `CHANGELOG.md` or the version in `package.json`.** The Release workflow writes both. A maintainer starts it by hand and reads every change to `.github/workflows/release.yml`.
- **Do not edit the Repo Concierge's files**: `.github/workflows/*.lock.yml` and the `.md` workflow sources beside them, `check-deployment.yml`, `concierge-updater.yml`, `dependency-watch.yml`, `janitor.yml`, `plan-approval-resume.yml`, `pr-ready-signal.yml`, `reporter-notify.yml`, the scripts of the same names (`check-deployment.mjs`, `concierge-updater.mjs`, `dependency-watch.mjs`, `janitor.mjs`, `plan-approval-resume.mjs`, `pr-ready-signal.mjs`, `reporter-notify.mjs`) in `.github/scripts/`, `.github/aw/`, `.gitattributes` and the pin in `.github/concierge.yml`. An update overwrites them. `decision-records.yml`, `validate-decision-records.*` in `.github/scripts/` and `.github/agentic-workflows/scope.md` belong to this repository.
- **Never commit credentials.** This repository is public.

## Decisions

Accepted records are binding for humans and agents until a newer record supersedes them. If your task conflicts with one, stop and say so in the issue or pull request. A pull request that changes a decided behaviour adds or supersedes a record. Decisions for this repository are in [docs/decisions/](docs/decisions/README.md): To write one, use the [record-a-decision](.claude/skills/record-a-decision/SKILL.md) skill.

- [0001](docs/decisions/0001-the-action-validates-a-candidate-release-and-changes-nothing.md): The action validates a candidate release and changes nothing.
- [0002](docs/decisions/0002-collisions-are-checked-by-default-and-drift-only-on-request.md): Collisions are checked by default and drift only on request.
- [0003](docs/decisions/0003-a-stable-base-is-compared-only-with-stable-releases.md): A stable base is compared only with stable releases.
- [0004](docs/decisions/0004-releases-run-through-one-dispatched-workflow-that-uses-the-sibling-actions.md): Releases run through one dispatched workflow that uses the sibling actions.
