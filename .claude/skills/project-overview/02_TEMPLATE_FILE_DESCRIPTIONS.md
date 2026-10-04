# {{Project Name}} — File Descriptions

> **Generated:** {{YYYY-MM-DD}} · **Last updated:** {{YYYY-MM-DD}}
> **Scope:** {{folders covered}} · **Excluded:** {{folders or file types left out}}

<!--
TEMPLATE INSTRUCTIONS
- This is action 1. It runs first and is built directly from the code.
- Follow 00_GENERATION_RULES.md (depth rule, update mode, cross-linking) and 01_LANGUAGE_GUIDE.md.
- Everything inside HTML comments is guidance. Remove it from the final document.
- Document by business value: full detail for code that creates, changes, or deletes business data, applies business rules, starts a flow, or calls an external service. One line for helpers. Leave out imports, internal dependencies, and plumbing.
-->

## How to Read This Document

<!--
Answer:
- How are the files ordered (by area or folder)?
- What do the labels mean? (keep the table below)
- What was left out, and why?
-->

{{One or two sentences on how the document is ordered and what it covers.}}

| Label | Meaning |
|---|---|
| 🚪 | Entry point: a request, page, job, or command starts here |
| 🗄️ | Creates, changes, deletes, or reads database data |
| 🌐 | Calls an external service (payments, email, third-party API) |
| ⏱️ | Runs on a schedule or in the background |

## Business Actions Index

<!--
Answer:
- For each main business entity (Customer, Order, Invoice, ...), where is it created, changed, and removed?
- One row per entity. Link each function to its section below.
- This table lets a reader answer "where are customers created?" at a glance.
- Write "Never" if the app never does that action.
-->

| Entity | Created in | Changed in | Removed in |
|---|---|---|---|
| {{Customer}} | [`{{create_customer()}}`](#file-{{anchor}}) | {{...}} | {{...}} |

## File Index

<!--
Answer:
- What files are documented, which area does each belong to, and what does each do in one line?
- List files in the same order as the sections below.
-->

| File | Area | Purpose |
|---|---|---|
| [`{{path/to/file}}`](#file-{{anchor}}) | {{Area}} | {{One line}} |

---

## {{Area or Folder Name}}

<!--
Answer:
- What does this group of files do together, in one or two sentences?
-->

{{Group summary.}}

<a id="file-{{path-with-dashes}}"></a>
### `{{path/to/file}}`

**Labels:** {{🚪 🗄️ 🌐 ⏱️ — only those that apply}}

**Purpose:** <!-- What is this file for? One or two plain sentences. --> {{...}}

**Role in the app:** <!-- Which part of the app is this (page, API, business logic, background job)? Which core flow step uses it? Link the step in PROJECT_OVERVIEW.md once it exists. --> {{...}}

**Data touched:** <!-- Which tables does it create, change, delete, or read? Link each to DATABASE_OVERVIEW.md. Write "None" if none. --> {{...}}

**External effects:** <!-- Does it send email, take payments, call outside APIs, or write files? Write "None" if none. --> {{...}}

#### `{{function_name}}({{inputs}})`

<!-- Repeat for each function worth documenting in full (see depth rule). -->

- **What it does:** <!-- One sentence. --> {{...}}
- **Inputs:** <!-- What goes in, in words: "the customer's email and chosen plan", not only types. --> {{...}}
- **Output:** <!-- What comes back, or what the result is. --> {{...}}
- **Side effects:** <!-- What changes outside the function: rows saved, emails sent, status changed. "None" if none. --> {{...}}
- **Triggered by:** <!-- Which user action, page, route, job, or other function calls this, in business terms. --> {{...}}
- **Worth knowing:** <!-- Only business rules, limits, or notable failure cases. Leave this line out if there is nothing. --> {{...}}

#### Class `{{ClassName}}`

<!-- Use this block instead of the function block for classes. -->

- **Represents:** <!-- What real-world thing or job does this class stand for? --> {{...}}
- **Key data it holds:** <!-- Only attributes that matter to the business. --> {{...}}
- **Methods:**
  - `{{method_name}}({{inputs}})` — <!-- What it does, what goes in, what comes out, any side effects. One or two sentences. --> {{...}}

**Notes:** <!-- Optional. Anything surprising: misleading names, legacy behaviour, known problems. Leave out if nothing. --> {{...}}

---

## Summarized Files

<!--
Files with little business value (helpers, utilities, setup, wiring).
One line each. No function details.
-->

| File | Purpose |
|---|---|
| `{{path/to/helper}}` | {{One line}} |
