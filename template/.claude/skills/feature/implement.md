# /feature implement

## When this runs

Invoked as `/feature implement @<spec-file>`, e.g.
`/feature implement @claude-context/12-password-reset-spec.md`. The
argument names the spec to build.

## Steps

1. **Find the spec.** Use the file named in the argument. If there's no
   argument, or the file doesn't exist, say so, list the files matching
   `claude-context/*-spec.md` whose **Status** is `New`, and ask which one to
   build. Don't pick one yourself. If the spec's **Status** is anything
   other than `New`, stop: that feature has already been built. Tell the
   user the next step for its status (`Implemented` → `/feature test`,
   `Tested` → `/feature finalize`, `PR created` or `Documented` → already
   finalized).
   Read the spec's **Mode** line and state it in one line (e.g. "Mode:
   SURF"). No **Mode** line means `VERBOSE`. See *Modes* in `SKILL.md`.
2. **Check the starting point.** Run `git status`. Every feature starts
   on `main`. If you're not on `main`, or anything besides the spec file
   is uncommitted, stop and ask the user what to do.
3. Read `CLAUDE.md` and `claude-context/ai-interaction.md` (the project's
   working rules, if not already fresh in context), the spec file itself
   — it may have been edited since it was created — and the code the
   spec touches. The code is always the source of truth. Read
   `docs/architecture.md` and `claude-context/Project Overview/` for
   orientation if useful; both can lag behind the code. Don't stop
   because they differ from the code or the spec. If the feature adds
   or changes anything the user sees, open every image in
   `claude-context/ui-prototypes/` (there are only a few).
4. **Build it**, following `ai-interaction.md`'s workflow:
   - Branch first, from an up-to-date `main` (steps 2–3 of
     `claude-context/project-git-workflow.md`): `git pull --ff-only`, then
     `git checkout -b <spec slug>`. The branch name is the spec's slug,
     e.g. `12-password-reset`. If a branch with that name already
     exists, stop and tell the user: a feature is built once, on a new
     branch. Show each command; in VERBOSE, wait for a go-ahead first.
   - Build the feature following existing code patterns in the codebase.
     Schema changes go through the migration tool, as `CLAUDE.md`
     describes under *Database migrations*.
   - UI follows the screenshots in `claude-context/ui-prototypes/` as
     closely as reasonable, without copying them pixel for pixel. A
     screen with a matching screenshot (the spec names it, or its file
     name matches) follows that one. Every other screen takes its look
     from all of them together. If matching a screenshot would need a new
     dependency or a big structural change, ask first.
   - Don't write or run automated tests, even where the spec lists tests
     to add. That's `/feature test`.
   - Explain what changed in the plain-language/junior-dev style
     `ai-interaction.md` describes, with one line on how each new or
     changed screen follows the screenshots and any deliberate
     difference, and end with the spec's **How to see
     it working:** line so the user can run it.
   - Before the first commit, change the spec's **Status** line to
     `Implemented`, so the spec goes onto the branch marked as built.
   - Commit on the feature branch: run `git status`, check the file list
     against *Never commit* in `CLAUDE.md` (a hit always stops, in both
     modes), then show the exact `git add -A` and commit commands. In
     VERBOSE, wait for approval; in SURF, run them. Commit as
     often as makes sense. Never add a `Co-Authored-By` line for Claude
     or any AI tool.
5. Everything else in `ai-interaction.md` applies too — ask before big
   structural decisions, don't add anything the spec didn't ask for,
   mention off-task cleanup ideas instead of doing them on the spot.
6. **Stop after the commit.** Don't write tests, push, open a PR, or
   merge — that's `/feature test`, then `/feature finalize`. If the user
   reports a problem while trying the feature by hand, fix it on this
   same branch, explain the fix, and commit it with the same rules.
   Don't push. When the user is happy with it, tell them `/feature test`
   is next.
