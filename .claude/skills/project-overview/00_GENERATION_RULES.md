# Documentation Generation Rules

These rules apply every time the documentation skill runs. They define what the skill produces, in what order, how it updates existing documents, and how much detail to include.

## Purpose

The skill writes three documents that explain a software project to two audiences:

- **Non-technical stakeholders**, who need to understand what the app does and what data it holds.
- **Junior developers**, who need to find their way around the code quickly.

Every document must be clear, short, and accurate. All writing follows `01_LANGUAGE_GUIDE.md`.

## Output Files

All output goes in the folder `context/Project Overview/`. This location is fixed and applies every time the skill runs.

- If the folder does not exist, create it.
- Never write the documents anywhere else, even if a `docs/` or similar folder already exists.
- The folder name contains a space. Quote the path in shell commands: `"context/Project Overview/"`.
- All three documents sit side by side in this folder, so cross-links between them are plain file names (for example `DATABASE_OVERVIEW.md#table-customers`).

| Order | Action | Output file | Template |
|---|---|---|---|
| 1 | `file-descriptions` | `context/Project Overview/FILE_DESCRIPTIONS.md` | `02_TEMPLATE_FILE_DESCRIPTIONS.md` |
| 2 | `database-overview` | `context/Project Overview/DATABASE_OVERVIEW.md` | `03_TEMPLATE_DATABASE_OVERVIEW.md` |
| 3 | `project-overview` | `context/Project Overview/PROJECT_OVERVIEW.md` | `04_TEMPLATE_PROJECT_OVERVIEW.md` |

## Order and Dependencies

Every run performs all three actions, in the order above. Each document builds on the ones before it:

1. **File Descriptions** comes first because it is built directly from the code. It records which functions create, change, and delete data.
2. **Database Overview** comes second. It uses File Descriptions to fill in where each table is written and read.
3. **Project Overview** comes last. It summarizes both earlier documents into the architecture and the core data flow.

Each action reads the earlier documents as they stand after this run, so they are always current.

**Core flow step numbers:** one source sets the step numbers, and all three documents use them. The first that applies wins:

1. The *Core flow* in `docs/architecture.md`, whether or not code exists.
2. Otherwise, *Data Through the Core Flow* in `DATABASE_OVERVIEW.md`, numbered fresh from the code in this run.
3. Otherwise (no database), *Core Data Flow* in `PROJECT_OVERVIEW.md`.

Never take step numbers from a document written in an earlier run. Links from an earlier document to a later one (for example File Descriptions → flow steps) are filled in by a link pass after all three are written.

### Projects Without a Database

If the project has no database, still write all three documents:

- `DATABASE_OVERVIEW.md` keeps only the title, metadata header and **At a Glance**. At a Glance says there is no database and explains where the app keeps its data instead (files, browser storage, an external API, or nowhere). Leave out every other section.
- `PROJECT_OVERVIEW.md` sets the core flow step numbers only if `docs/architecture.md` has no *Core flow* (see *Core flow step numbers*). Under *Database Changes* it writes "Not relevant at this point." instead of the link to `DATABASE_OVERVIEW.md`.
- File Descriptions and Project Overview leave out table links. The 🗄️ label marks code that reads or writes stored data, wherever it lives.

### Projects Without Code Yet

A new project may have a design in `docs/architecture.md` and a phase plan in `docs/project-phase-plan.md` (both written by `/setup`) but little or no code. Still write all three documents:

- Take entities, fields, relationships, statuses, the core flow and the architecture from `docs/architecture.md`, and the phases from `docs/project-phase-plan.md`. Mark each one that has no code yet with **🛠️ Planned**.
- `DATABASE_OVERVIEW.md` describes the planned entities as tables. Leave the *Lifecycle* "Where" column as `🛠️ Planned` until code exists.
- `FILE_DESCRIPTIONS.md` describes only files that exist, such as the scaffolded starter files.
- Reuse the core flow step numbers from `docs/architecture.md`.

## Update Mode

The skill updates existing documents. It does not rewrite them from scratch.

If the output file does not exist yet, create it in full from the template.

If the output file already exists:

1. Compare every section against the project's files as they are on disk now, which match the latest `main` (see Step 0 in `SKILL.md`). Do not use git history to decide what changed.
2. A section is affected if anything it describes no longer matches the code. Added, removed, and renamed files, functions, and tables each count as changes.
3. Rewrite only the affected sections. Keep every unaffected section word for word.
4. Never change text between `<!-- manual:start -->` and `<!-- manual:end -->`. People write these sections by hand.
5. Keep each `⚠️ Needs confirmation` marker until the code or the user answers the question.
6. Fix any cross-links that point to renamed or removed items.
7. If any section in this document changed, update the last updated date in the metadata header. Otherwise leave the whole document as it is, header included: a new date alone is not a change.
8. Tell the user what changed, in a short list.

## Depth Rule: Focus on Business Value

Document code by how much it explains what the app does with its data, not by how much code there is.

**The test:** Would a product owner or a new developer need this to understand how the app handles its business data? If yes, document it in full. If no, give it one line or leave it out.

**Document in full:**
- Code that creates, changes, or deletes a business entity (for example, where a Customer is created).
- Code that applies a business rule (pricing, eligibility, status changes, limits).
- Entry points: routes, pages, scheduled jobs, event handlers, CLI commands.
- Code that talks to an external service (payments, email, third-party APIs).
- Each step of the core data flow.

**Summarize in one line:**
- Helpers and utilities (formatting, parsing, small wrappers).
- Framework setup and wiring (app start-up, dependency setup, middleware).

**Leave out:**
- Tests, generated code, vendored or third-party code, build output.
- Library imports, internal technical dependencies, and implementation details that do not change what the business sees. Describe *what* happens to the data, not *how* the code is plumbed together.

If you are unsure, ask: "Does this change what happens to a customer, an order, or another business entity?" If not, summarize it.

## Accuracy

- Base every statement on the code. Do not guess business intent.
- If the code does not make something clear, write `> ⚠️ Needs confirmation: <question>` instead of guessing.
- Never include secret values, passwords, keys, or real personal data. Environment variable *names* are fine.
- Do not describe features, files, or tables that do not exist in the code. The one exception: items that `docs/architecture.md` plans but the code does not have yet, marked **🛠️ Planned** (see *Projects Without Code Yet*). Where the code and `docs/architecture.md` both describe something, the code wins.
- Remove the 🛠️ Planned mark from an item as soon as the code has it.

## Cross-Linking

The three documents link to each other. Use explicit anchors so links stay stable:

| Item | Anchor format | Example |
|---|---|---|
| File | `file-<path>` with `/` and `.` replaced by `-` | `<a id="file-src-services-customer-py"></a>` |
| Table | `table-<table_name>` | `<a id="table-customers"></a>` |
| Core flow step | `flow-step-<number>` | `<a id="flow-step-3"></a>` |

Place the anchor on the line directly above the heading it belongs to. Link with `[customers](DATABASE_OVERVIEW.md#table-customers)`.

Required links:
- File Descriptions → tables each file touches, and flow steps each file takes part in.
- Database Overview → files that create, change, delete, and read each table.
- Project Overview → files and tables for each core flow step.

## Metadata Header

Every document starts with this header, directly under the title:

```markdown
> **Generated:** YYYY-MM-DD · **Last updated:** YYYY-MM-DD
> **Scope:** <folders covered> · **Excluded:** <folders or file types left out>
```

## Diagrams

Write all diagrams in Mermaid inside a ```` ```mermaid ```` block. Keep each diagram small enough to read without zooming. If a diagram would need more than about 15 boxes, split it or show only the main parts.

## Final Check Before Saving

- [ ] Every section in the template is filled in, or marked `⚠️ Needs confirmation`.
- [ ] The writing follows every rule in `01_LANGUAGE_GUIDE.md`.
- [ ] All cross-links point to anchors that exist.
- [ ] The metadata header is updated if any section changed, and untouched otherwise.
- [ ] Unchanged sections and manual sections are untouched.
