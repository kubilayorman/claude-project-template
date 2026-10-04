# /feature implement

## When this runs

Invoked as `/feature implement @<spec-file>`, e.g.
`/feature implement @context/12-password-reset-spec.md`. The
argument names the spec to build.

## Steps

1. **Find the spec.** Use the file named in the argument. If there's no
   argument, or the file doesn't exist, say so, list the files matching
   `context/*-spec.md` whose **Status** is `New`, and ask which one to
   build. Don't pick one yourself. If the spec's **Status** is
   `PR created`, stop: that feature has already been built and
   finalized.
2. **Check the starting point.** Run `git status`. Every feature starts
   on `main`. If you're not on `main`, or anything besides the spec file
   is uncommitted, stop and ask the user what to do.
3. Read `CLAUDE.md` and `context/ai-interaction.md` (the project's
   working rules, if not already fresh in context), the spec file itself
   — it may have been edited since it was created — and the code the
   spec touches. The code is always the source of truth. Read
   `docs/architecture.md` and `context/Project Overview/` for
   orientation if useful; both can lag behind the code. Don't stop
   because they differ from the code or the spec.
4. **Build it**, following `ai-interaction.md`'s workflow:
   - Branch first, from an up-to-date `main` (steps 2–3 of
     `context/project-git-workflow.md`): `git pull --ff-only`, then
     `git checkout -b <spec slug>`. The branch name is the spec's slug,
     e.g. `12-password-reset`. If a branch with that name already
     exists, stop and tell the user: a feature is built once, on a new
     branch. Show each command and wait for go-ahead.
   - Build the feature following existing code patterns in the codebase.
     Schema changes go through the migration tool, as `CLAUDE.md`
     describes under *Database migrations*.
   - Explain what changed in the plain-language/junior-dev style
     `ai-interaction.md` describes, and end with the spec's **How to see
     it working:** line so the user can run it.
   - Commit on the feature branch: run `git status`, check the file list
     against *Never commit* in `CLAUDE.md`, then show the exact
     `git add -A` and commit commands and wait for approval. Commit as
     often as makes sense. Never add a `Co-Authored-By` line for Claude
     or any AI tool.
5. Everything else in `ai-interaction.md` applies too — ask before big
   structural decisions, don't add anything the spec didn't ask for,
   mention off-task cleanup ideas instead of doing them on the spot.
6. **Stop after the commit.** Don't push, open a PR, or merge — that's
   `/feature finalize`. If the user reports a problem while testing, fix
   it on this same branch, explain the fix, and commit it with the same
   rules. Don't push.
