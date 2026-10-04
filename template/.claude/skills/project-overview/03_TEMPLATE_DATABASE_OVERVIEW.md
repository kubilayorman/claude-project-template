# {{Project Name}} — Database Overview

> **Generated:** {{YYYY-MM-DD}} · **Last updated:** {{YYYY-MM-DD}}
> **Scope:** {{schema, models, and migration folders covered}} · **Excluded:** {{...}}

<!--
TEMPLATE INSTRUCTIONS
- This is action 2. It runs after `claude-context/Project Overview/FILE_DESCRIPTIONS.md` has been written or updated in the same run.
- Use FILE_DESCRIPTIONS.md (especially the Business Actions Index) to fill in where each table is created, changed, deleted, and read. Confirm against the code.
- Follow 00_GENERATION_RULES.md and 01_LANGUAGE_GUIDE.md.
- Everything inside HTML comments is guidance. Remove it from the final document.
-->

## At a Glance

<!--
Answer:
- What does the data represent overall? One plain-language paragraph.
- Which database engine is used, and where is it hosted?
- Which tool defines the tables and runs migrations (ORM or migration tool)?
- How many tables are there?
-->

{{One paragraph a non-technical reader can follow.}}

| | |
|---|---|
| Database | {{PostgreSQL 16, hosted on ...}} |
| Schema defined in | {{ORM / migration tool, and folder}} |
| Number of tables | {{n}} |

## Entity Relationship Diagram

<!--
Answer:
- How do all tables connect? Show every table and relationship.
- If there are more than about 15 tables, show the main ones here and note which are left out.
-->

```mermaid
erDiagram
    {{CUSTOMERS}} ||--o{ {{ORDERS}} : places
```

## Tables

<!-- Repeat the block below for each table. Order tables so related ones sit together, main business tables first. -->

<a id="table-{{table_name}}"></a>
### `{{table_name}}`

**Purpose:** <!-- Why does this table exist? One sentence. --> {{...}}

**One row represents:** <!-- For example: "one order placed by a customer". --> {{...}}

**Fields:**

<!-- Describe each field in business terms. Put constraints (unique, default, allowed values) in the last column. -->

| Field | Type | Required | Description | Example / Constraints |
|---|---|---|---|---|
| `{{id}}` | {{uuid}} | Yes | {{...}} | {{Primary key}} |

**Keys and indexes:** <!-- Primary key, foreign keys, unique constraints. Indexes only if they exist for a clear reason; say the reason. --> {{...}}

**Relationships:** <!-- Plain language first, then the technical form. Example: "One customer has many orders (1:N via `orders.customer_id`)." --> {{...}}

**Lifecycle:**

<!-- Link each function to FILE_DESCRIPTIONS.md. Write "Never" where the app does not do that action. -->

| Action | Where | When |
|---|---|---|
| Created | [`{{function()}}`](FILE_DESCRIPTIONS.md#file-{{anchor}}) | {{Which user action or flow step}} |
| Changed | {{...}} | {{...}} |
| Deleted | {{...}} | {{Soft or hard delete? What else gets deleted with it?}} |
| Read | {{...}} | {{Which features use it}} |

**Status values:** <!-- Only if the table has a status-like field. List allowed values and how a row moves between them. Add a small state diagram if there are more than three states. Leave out otherwise. -->

```mermaid
stateDiagram-v2
    [*] --> {{pending}}
    {{pending}} --> {{paid}}
```

**Sensitive data:** <!-- Which fields hold personal or secret data? Write "None" if none. --> {{...}}

**Worth knowing:** <!-- Optional. Misleading names, legacy fields, unused columns. Leave out if nothing. --> {{...}}

---

## Relationships Summary

<!--
Answer:
- What are all the relationships in one place?
- For each many-to-many relationship, what does the linking table mean in business terms?
-->

| From | To | Type | Via | On delete |
|---|---|---|---|---|
| `{{customers}}` | `{{orders}}` | 1:N | `{{orders.customer_id}}` | {{Cascade / Restrict / Set null}} |

{{One sentence per many-to-many linking table explaining what it represents.}}

## Data Through the Core Flow

<!--
Answer:
- As the main flow runs, which tables are created (C), read (R), updated (U), or deleted (D) at each step?
- Rows are flow steps; columns are tables. Step numbers follow "Core flow step numbers" in 00_GENERATION_RULES.md.
- Follow the table with two or three sentences describing the flow of data in plain language.
-->

| Step | `{{table_a}}` | `{{table_b}}` | `{{table_c}}` |
|---|---|---|---|
| 1. {{Customer signs up}} | C | | |
| 2. {{Customer places order}} | R | C | |

{{Plain-language summary of how data moves through the flow.}}

## Migrations and Schema Changes

<!--
Answer:
- Where do migrations live?
- How do you create a new migration and run it? Give the exact commands.
- What naming or ordering conventions does the project use?
-->

{{...}}
