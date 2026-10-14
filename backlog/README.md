# Backlog

## Structure

```
backlog/
├── items/       # one .md file per open or in-progress item
├── archive/     # one .md file per closed item (DONE or CANCELED)
├── _runs/       # ephemeral agent run artifacts (gitignored)
├── _template.md # copy when creating a new item
└── README.md    # this file
```

Ticket IDs use `{PROJECT_KEY}-{NNN}` (e.g. `APP-001` when `PROJECT_KEY` is `APP`). IDs MUST NOT be reused, even if an item is cancelled. Filenames match the ticket ID (e.g. `APP-001.md`).

## Statuses

### In `items/`

| Status | Meaning | Who may set |
|--------|---------|-------------|
| `OPEN` | Not approved for agents. | Operator |
| `READY` | Approved and eligible for pickup. | Operator |
| `ON-HOLD` | Explicitly paused. | Operator |
| `IN_PROGRESS` | Claimed by an agent; running or resumable. | Agent (on start); operator may reset |
| `AWAITING_OPERATOR` | Agent work complete; pending operator review. | Agent (on complete) |
| `CANCELED` | Discarded. Revert partial work if implementation had started. | Operator |

### In `archive/`

| Status | Meaning | Who may set |
|--------|---------|-------------|
| `DONE` | Accepted by operator. File moved to `archive/`. | **Operator only** |

## Frontmatter schema

### Required fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | `{PROJECT_KEY}-{NNN}` — zero-padded sequential integer |
| `title` | string | Short headline (≤ 100 characters), imperative mood |
| `status` | enum | See status tables above |
| `type` | enum | `feature`, `chore`, `docs`, `test`, `setup`, `infra` |
| `priority` | enum | `must`, `should`, `could` (MoSCoW) |
| `milestone` | string | e.g. `M1`, `M2` — per project convention |
| `dependencies` | list | Ticket IDs that must be `AWAITING_OPERATOR` or `DONE` before this item starts. Default `[]`. |

### Optional fields

| Field | Type | Description |
|-------|------|-------------|
| `order` | float | Visual sort position within a priority column. Set by the Scaffold app or operator — agents read it for sorting but never assign or modify it. Lower value = higher on the board. |
| `references.adrs` | list | ADR filenames this item relates to |
| `references.prds` | list | PRD filenames this item relates to |
| `references.docs` | list | Other doc paths relevant to this item |

## Markdown body

Below the frontmatter, each item has three sections:

- **Description** — 1–5 sentences: problem, goal, scope.
- **Acceptance Criteria** — checkable conditions for `AWAITING_OPERATOR`. Must be testable and observable.
- **Notes** — optional free-form context, open questions, or hints.

Use `_template.md` when creating a new item.
