---
name: project-overview
description: Slash command only — /project-overview — writes or updates the onboarding docs in `context/Project Overview/` (FILE_DESCRIPTIONS.md, DATABASE_OVERVIEW.md, PROJECT_OVERVIEW.md) for non-technical stakeholders and junior developers, and opens a PR for them. Runs only from an up-to-date `main` with no open PRs. Do not trigger from plain conversation without /project-overview.
---

# Project overview

Brings three linked documents in `context/Project Overview/` up to date. The rules live in the files next
to this one:

- `00_GENERATION_RULES.md` — outputs, order, update mode, depth, cross-links, metadata. **Read every run.**
- `01_LANGUAGE_GUIDE.md` — writing rules. **Read every run.**
- `02_` / `03_` / `04_TEMPLATE_*.md` — one template per document.

If this file and a numbered file disagree, the numbered file wins.

## Every run does the same thing

There are no arguments. Each run creates or updates all three documents, in this order:

1. `FILE_DESCRIPTIONS.md`, from the code.
2. `DATABASE_OVERVIEW.md`, from the code plus the file descriptions just written.
3. `PROJECT_OVERVIEW.md`, from the code plus both documents above.
4. A link pass across all three.

Each document reads the earlier ones as they stand after this run, so none can be stale. Update
mode compares each section against the code on disk and rewrites only the sections that no
longer match.

## Step 0 — Check the starting point

The documents are a snapshot of the latest `main`, so this never runs while a feature is in
progress. Show each git and `gh` command and wait for a go-ahead. Stop and tell the user why if
any check below fails, or if the developer declines one of its commands:

1. `git status`: you are on `main` and the working tree is clean (no uncommitted or untracked
   files, which would include a spec from `/feature describe`).
2. `git pull --ff-only`: local `main` now matches `main` on GitHub. If it fails, point the user to
   *Troubleshooting* in `context/project-git-workflow.md`.
3. `gh pr list`: no open PRs. Every PR must be approved and merged first, so the snapshot
   includes all work in progress.

## Step 1 — Read the project

- Start with the project's own words: `README.md`, `CLAUDE.md`, and the docs they point to. The
  code is the source of truth; trust it, then the design docs `CLAUDE.md` lists under *Source of
  truth*, over issues and comments.
- If `docs/architecture.md` describes things the code doesn't have yet, include them marked 🛠️ Planned, as 00's
  *Projects Without Code Yet* explains. `docs/project-phase-plan.md` feeds only the *Phases* section of
  `PROJECT_OVERVIEW.md`.
- Traverse the whole tree fresh each run, excluding `.git`, dependency folders, build output,
  lockfiles, `context/Project Overview/`, and the template's own folders: `.claude/`, `context/`,
  `docs/` and `project-type/`. Those are not app code, so they don't get file descriptions.
  Follow imports out from the entry points.
- Take the "business entities" for 00's depth rule from the project's own domain model, not the
  templates' customer/order examples.
- Read the project's files as they are on disk. Step 0 made sure they match the latest `main` on
  GitHub, so the documents describe `main` as it is right now.

## Step 2 — Write or update each document

For each document in order: create it from its template, or follow 00's Update Mode. Strip all
template HTML comments. Run 00's Final Check and 01's Final Language Check before saving.

Two rules the templates leave implicit:

- **What → how → why.** Every non-obvious choice gets a substantive reason. Where the project
  picked the unusual option, name the usual alternative and what goes wrong with it.
- **Explain, don't enumerate.** Architecture, flow and workflow sections are paragraphs. Bullets
  are for enumerable things: fields, commands, files.

## Step 3 — Link pass

`FILE_DESCRIPTIONS.md` is written before the tables and flow steps it links to. After all three
documents are written, add any missing links to `DATABASE_OVERVIEW.md#table-*` and
`PROJECT_OVERVIEW.md#flow-step-*`, then check that every anchor linked from any document exists.

## Step 4 — Open a PR

If no document changed, skip this step. A new date alone doesn't count as a change (see 00's Update Mode). Otherwise follow Part 1 of
`context/project-git-workflow.md`, showing each command and waiting for a go-ahead:

1. `git checkout -b project-overview-YYYY-MM-DD` (today's date).
2. `git add "context/Project Overview/"`, then commit with a message like
   `docs: update Project Overview`. Only this folder is committed.
3. Run the checks in `CLAUDE.md`, then `git push -u origin <branch>`.
4. `gh pr create` with the title `Project Overview update YYYY-MM-DD` and a body that lists, per
   document, the sections changed. No `Closes` line.
5. `git checkout main`.

No commit message and no PR title or body may credit Claude or any AI tool.

## Step 5 — Report, then stop

Per document: created, updated (list the sections changed), or unchanged. Then list every place
where the code differs from `docs/architecture.md` or `docs/project-phase-plan.md` (not counting
items still 🛠️ Planned), for the user's information. Don't edit the design docs.
End with the PR link and one line: a teammate approves and merges it on GitHub. If no document
changed, say instead that no PR was needed. No summary, no offers, no follow-up questions.

Other files in `context/Project Overview/` (e.g. `project-overview-YYYY-MM-DD.md`, `db-overview.md`) are
not managed by this skill: never edit or delete them.
