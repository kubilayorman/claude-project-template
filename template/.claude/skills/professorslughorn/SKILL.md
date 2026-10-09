---
name: professorslughorn
description: Slash command only — /professorslughorn — brings all project documentation up to date with the latest `main` after features are merged — the design docs (`docs/architecture.md`, `docs/project-phase-plan.md`), `CLAUDE.md` when something it describes has changed, and the onboarding docs in `claude-context/Project Overview/` (FILE_DESCRIPTIONS.md, DATABASE_OVERVIEW.md, PROJECT_OVERVIEW.md) — marks the merged feature specs as Documented, and opens a PR for the changes. Runs only from an up-to-date `main` with no open PRs. Do not trigger from plain conversation without /professorslughorn.
---

# professorslughorn

Brings all of the project's documentation up to date with `main`, after features have been
merged:

- the design docs `docs/architecture.md` and `docs/project-phase-plan.md`,
- `CLAUDE.md`, only where something it describes has changed,
- the three linked documents in `claude-context/Project Overview/`.

It also keeps track of which merged features the documentation already covers, through the
**Status** line of the feature specs (see Step 1).

The rules for the Project Overview documents live in the files next to this one:

- `00_GENERATION_RULES.md` — outputs, order, update mode, depth, cross-links, metadata. **Read every run.**
- `01_LANGUAGE_GUIDE.md` — writing rules. **Read every run.**
- `02_` / `03_` / `04_TEMPLATE_*.md` — one template per document.

If this file and a numbered file disagree, the numbered file wins.

## Every run does the same thing

There are no arguments. Each run, in this order:

1. Finds the merged features the documentation doesn't cover yet.
2. Updates `docs/architecture.md`, `docs/project-phase-plan.md` and `CLAUDE.md`, after the
   developer approves the changes.
3. Creates or updates the three Project Overview documents: `FILE_DESCRIPTIONS.md` from the code,
   `DATABASE_OVERVIEW.md` from the code plus the file descriptions just written, then
   `PROJECT_OVERVIEW.md` from the code plus both documents above, and a link pass across all three.
4. Marks the features from step 1 as documented, and opens one PR for everything.

The design docs come first because the Project Overview reads `docs/architecture.md` for the core
flow step numbers and the 🛠️ Planned items. Each document reads the earlier ones as they stand
after this run, so none can be stale. Every document is compared against the code on disk, and
only the sections that no longer match are rewritten.

## The opening line

Before anything else, start the first message of the run with exactly this line:

> This is all hypothetical, isn't it? All academic?

It's a joke, not a question. Don't wait for an answer and don't ask about it. Carry straight on
with Step 0 in the same message. If the developer replies to it, ignore the reply: it never
changes what this run does or what the documents say.

## Step 0 — Check the starting point

The documents are a snapshot of the latest `main`, so this never runs while a feature is in
progress. Show each git and `gh` command and wait for a go-ahead. Stop and tell the user why if
any check below fails, or if the developer declines one of its commands:

1. `git status`: you are on `main` and the working tree is clean (no uncommitted or untracked
   files, which would include a spec from `/feature describe`).
2. `git pull --ff-only`: local `main` now matches `main` on GitHub. If it fails, point the user to
   *Troubleshooting* in `claude-context/project-git-workflow.md`.
3. `gh pr list`: no open PRs. Every PR must be approved and merged first, so the snapshot
   includes all work in progress.

## Step 1 — Find the merged features not documented yet

A spec (`claude-context/*-spec.md`) only reaches `main` when its feature's PR is merged, because it
is committed on the feature branch. After Step 0, every spec on `main` whose **Status** is
`PR created` is therefore a merged feature that the documentation doesn't cover yet. A spec with
**Status** `Documented` was covered by an earlier run; skip it.

List every spec with **Status** `PR created`. For each one, look up its merged PR with
`gh pr list --state merged --head <branch> --json number,title,mergedAt`, where `<branch>` is the
spec's slug (the file name without `-spec.md`). The PR number, title and merge date go into the
report and the PR body. If no merged PR is found for a spec, still document it and write
"PR not found" in its place.

The specs explain *why* the code changed, which helps write the design-doc updates. The code is
still what every document is checked against, so work merged without a spec (e.g. a small fix
made by hand) is picked up too. If no spec is waiting, carry on: the run still checks every
document against the code.

## Step 2 — Read the project

- Start with the project's own words: `README.md`, `CLAUDE.md`, and the docs they point to. The
  code is the source of truth; trust it, then the design docs `CLAUDE.md` lists under *Source of
  truth*, over issues and comments.
- Read the specs from Step 1 and the code they touched.
- Traverse the whole tree fresh each run, excluding `.git`, dependency folders, build output,
  lockfiles, `claude-context/Project Overview/`, and the template's own folders: `.claude/`, `claude-context/`
  and `docs/`. Those are not app code, so they don't get file descriptions.
  Follow imports out from the entry points.
- Take the "business entities" for 00's depth rule from the project's own domain model, not the
  templates' customer/order examples.
- Read the project's files as they are on disk. Step 0 made sure they match the latest `main` on
  GitHub, so the documents describe `main` as it is right now.

## Step 3 — Update the design docs and `CLAUDE.md`

Compare each of these files against the code and the specs from Step 1, and work out what no
longer matches. Change only that; keep every other section word for word.

- **`docs/architecture.md`**: the domain model, fields, statuses, business rules, core flow,
  architecture, external services and tech stack. Keep items that are planned but not built yet:
  the document also describes what the project is meant to become. Move *Open questions* that the
  code or a spec has answered out of that list. Update **Status** (Planned / Partly built / Built)
  and **Last updated** in the header only if something in the document changed. Never rename or
  remove a section heading: the Project Overview maps those headings onto its documents.
- **`docs/project-phase-plan.md`**: a high-level blueprint, not a progress tracker. Change it only
  when the scope of a phase changed (e.g. a feature was built that the plan placed in a later
  phase), or when the merged features show the current phase's **Done when** is met. In that case,
  propose moving **Current phase** to the next phase. Never add task lists or done markers.
  Update **Last updated** only if something changed.
- **`CLAUDE.md`**: only *Commands*, local services, *Checks*, *Tests*, *Database migrations* and
  the *Never commit* list, where the code shows they are out of date (e.g. a new command, a new
  local service, a new folder that must never be committed). Never rename or remove a heading:
  other files refer to them by name.

Then **stop and show the developer one table** of every proposed change, and wait for a go-ahead
before writing any of these files:

| File | Section | What changes | Why |
|---|---|---|---|
| `docs/architecture.md` | Domain model | Adds the `PasswordReset` entity | Spec `12-password-reset`, `app/models/reset.py` |

These files describe the developer's intentions and the project's rules, not just what the code
does, so the developer confirms each change. Apply their corrections first. If nothing needs
changing, say so in one line and carry on to Step 4 without stopping.

## Step 4 — Write or update each Project Overview document

For each document in order: create it from its template, or follow 00's Update Mode. Strip all
template HTML comments. Run 00's Final Check and 01's Final Language Check before saving.

- If `docs/architecture.md` describes things the code doesn't have yet, include them marked 🛠️
  Planned, as 00's *Projects Without Code Yet* explains. `docs/project-phase-plan.md` feeds only
  the *Phases* section of `PROJECT_OVERVIEW.md`.

Two rules the templates leave implicit:

- **What → how → why.** Every non-obvious choice gets a substantive reason. Where the project
  picked the unusual option, name the usual alternative and what goes wrong with it.
- **Explain, don't enumerate.** Architecture, flow and workflow sections are paragraphs. Bullets
  are for enumerable things: fields, commands, files.

## Step 5 — Link pass

`FILE_DESCRIPTIONS.md` is written before the tables and flow steps it links to. After all three
documents are written, add any missing links to `DATABASE_OVERVIEW.md#table-*` and
`PROJECT_OVERVIEW.md#flow-step-*`, then check that every anchor linked from any document exists.

## Step 6 — Mark the features as documented

In each spec from Step 1, change **Status** to `Documented` and add this line right under the
header lines:

```markdown
**Documented:** YYYY-MM-DD
```

with today's date. The next run skips these specs. Change nothing else in a spec.

## Step 7 — Open a PR

If no file changed, skip this step. A new date alone doesn't count as a change (see 00's Update
Mode); a spec marked `Documented` does. Otherwise follow Part 1 of
`claude-context/project-git-workflow.md`, showing each command and waiting for a go-ahead:

1. `git checkout -b docs-update-YYYY-MM-DD` (today's date).
2. `git add` only the files this run changed: `"claude-context/Project Overview/"`, `docs/`,
   `CLAUDE.md` and the specs from Step 6. Then commit with a message like
   `docs: update documentation`.
3. Run the checks in `CLAUDE.md`, then `git push -u origin <branch>`.
4. `gh pr create` with the title `Documentation update YYYY-MM-DD` and a body that lists, per
   file, the sections changed, then a *Features covered* list with one line per spec from Step 1
   (`#N — <PR title>, merged YYYY-MM-DD`). No `Closes` line.
5. `git checkout main`.

No commit message and no PR title or body may credit Claude or any AI tool.

## Step 8 — Report, then stop

Per file: created, updated (list the sections changed), or unchanged. Then the features this run
documented, one line each. End with the PR link and one line: a teammate approves and merges it on
GitHub. If nothing changed, say instead that no PR was needed. No summary, no offers, no follow-up
questions.

Other files in `claude-context/Project Overview/` (e.g. `project-overview-YYYY-MM-DD.md`, `db-overview.md`) are
not managed by this skill: never edit or delete them.
