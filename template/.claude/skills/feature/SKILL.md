---
name: feature
description: Slash command only, with four sub-actions — /feature describe "<prompt>", /feature implement @<spec-file>, /feature test, /feature finalize — used to plan, build, test, and ship a feature or GitHub issue in this project. Do not trigger from plain conversation without one of these exact sub-actions.
---

# feature

Router for the feature workflow. Each action is a separate,
self-contained instruction file in this same folder. Read only the one
that matches what was typed, and follow it exactly — don't read the
others unless that file says to.

| Command | Action file | What it does |
|---|---|---|
| `/feature describe "<prompt>"` | [describe.md](describe.md) | Turns the prompt into a spec file under `claude-context/`. |
| `/feature implement @<spec-file>` | [implement.md](implement.md) | Builds the spec file named in the argument. No tests. |
| `/feature test` | [test.md](test.md) | Writes unit tests for the parts worth testing, after the user approves a test plan, and runs the test suite. |
| `/feature finalize` | [finalize.md](finalize.md) | Runs the non-test checks, marks the spec done, pushes the branch, and opens a PR for a teammate to approve. No tests. |

## Routing rule

Look at the first word typed after `/feature`:

- `describe` → read and follow `describe.md`. Everything after the word
  `describe` is the feature prompt.
- `implement` → read and follow `implement.md`. Everything after the
  word `implement` is the spec file.
- `test` → read and follow `test.md`.
- `finalize` → read and follow `finalize.md`.
- Anything else, or nothing at all → ask the user which of the four
  they meant. Don't guess.

These four always run in this order across a feature's lifecycle:
describe → implement → test → finalize, once each per feature (`test`
runs again if `finalize` sends the user back to it).

Only `/feature test` writes or runs automated tests. The spec's
**Status** line tracks where a feature is: `New` (describe) →
`Implemented` (implement) → `Tested` (test) → `PR created` (finalize). Never skip ahead
to a later action just because the conversation sounds ready for it —
each one only starts from its own explicit `/feature ...` invocation.

The developer's workflow ends when `/feature finalize` opens the PR.
Approval and merging happen on GitHub, by the reviewer. If the reviewer
requests changes instead, that is not a `/feature` action: it follows
Part 2 of `claude-context/project-git-workflow.md`.
