<!--
TEMPLATE — layout for docs/project-phase-plan.md
Used by /setup. Fill every {{placeholder}} from the user's answers. If the project already has
real work in it, the first phase sums up what is already built.
- This is a high-level blueprint: what each phase delivers, why, and in which order. It is not a
  task list. No checkboxes, no task lists, no per-item done markers; what is built is read from
  the code.
- Use the same names for entities and steps as docs/architecture.md, or as the code if there is no
  docs/architecture.md.
- Never guess. Anything unanswered goes under "Open questions".
- Delete this comment and every other template comment.
-->

# {{Project name}} — Phase Plan

> **Current phase:** {{1 — First version}} · **Last updated:** {{YYYY-MM-DD}}

This document is the high-level blueprint for building the project up, phase by phase. The design
itself (domain model, architecture, tech stack) is in `docs/architecture.md`. Change this plan only
when the shape of a phase changes, not to track progress.

## Phases at a glance

| Phase | Goal |
|---|---|
| {{1 — First version}} | {{One sentence, in business terms.}} |
| {{2 — Later}} | {{...}} |

<!-- Repeat this block for each phase, in order. -->

## {{1 — First version}}

**Goal:** {{One or two sentences: why this phase comes first and what it makes possible.}}

**Includes:**

- {{A capability in business terms, e.g. "Customers book a table online." Name the entities and
  core flow steps from docs/architecture.md it covers.}}

**Leaves for later:** {{What deliberately waits, and for which phase.}}

**Done when:** {{One or two sentences: what a user can do at the end of this phase.}}

## Open questions

<!-- Questions about scope or order skipped or left unclear during setup. Write "None" if none. -->

- {{...}}
