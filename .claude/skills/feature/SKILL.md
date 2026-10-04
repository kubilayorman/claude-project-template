---
name: feature
description: Slash command only, with three sub-actions — /feature describe "<prompt>", /feature implement @<spec-file>, /feature finalize — used to plan, build, and ship a feature or GitHub issue in this project. Do not trigger from plain conversation without one of these exact sub-actions.
---

# feature

Router for the feature workflow. Each action is a separate,
self-contained instruction file in this same folder. Read only the one
that matches what was typed, and follow it exactly — don't read the
others unless that file says to.

| Command | Action file | What it does |
|---|---|---|
| `/feature describe "<prompt>"` | [describe.md](describe.md) | Turns the prompt into a spec file under `context/`. |
| `/feature implement @<spec-file>` | [implement.md](implement.md) | Builds the spec file named in the argument. |
| `/feature finalize` | [finalize.md](finalize.md) | Runs checks, marks the spec done, pushes the branch, and opens a PR for a teammate to approve. |

## Routing rule

Look at the first word typed after `/feature`:

- `describe` → read and follow `describe.md`. Everything after the word
  `describe` is the feature prompt.
- `implement` → read and follow `implement.md`. Everything after the
  word `implement` is the spec file.
- `finalize` → read and follow `finalize.md`.
- Anything else, or nothing at all → ask the user which of the three
  they meant. Don't guess.

These three always run in this order across a feature's lifecycle:
describe → implement → finalize, once each per feature. Never skip ahead
to a later action just because the conversation sounds ready for it —
each one only starts from its own explicit `/feature ...` invocation.

The developer's workflow ends when `/feature finalize` opens the PR.
Approval and merging happen on GitHub, by the reviewer. If the reviewer
requests changes instead, that is not a `/feature` action: it follows
Part 2 of `context/project-git-workflow.md`.
