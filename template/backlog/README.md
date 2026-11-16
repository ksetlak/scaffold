# Backlog

## Structure

```
backlog/
├── items/       # one .md file per open or in-progress item
├── archive/     # one .md file per closed item (DONE or CANCELED)
├── private/     # confidential bodies of security tickets (gitignored)
├── _runs/       # ephemeral agent run artifacts (gitignored)
├── _template.md # copy when creating a new item
└── README.md    # this file
```

Ticket IDs use `{PROJECT_KEY}-{NNN}` (e.g. `APP-001` when `project_key` in `scaffold.yaml` is `APP`). The number is zero-padded to `ticket_id_digits` in `scaffold.yaml` (default 3, so `APP-001`; 4 gives `APP-0001`). IDs MUST NOT be reused, even if an item is cancelled. Filenames match the ticket ID (e.g. `APP-001.md`).

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
| `id` | string | `{PROJECT_KEY}-{NNN}` — sequential integer, zero-padded to `ticket_id_digits` (default 3) |
| `title` | string | Short headline (≤ 100 characters), imperative mood |
| `status` | enum | See status tables above |
| `type` | enum | `feature`, `bug`, `refactor`, `perf`, `security`, `chore`, `docs`, `test`, `setup`, `infra`. `security` tickets follow [Security tickets](#security-tickets). |
| `priority` | enum | `must`, `should`, `could` (MoSCoW) |
| `milestone` | string | e.g. `M1`, `M2` — per project convention |
| `dependencies` | list | Ticket IDs that must be `AWAITING_OPERATOR` or `DONE` before this item starts. Default `[]`. |

### Optional fields

| Field | Type | Description |
|-------|------|-------------|
| `references.adrs` | list | ADR filenames this item relates to |
| `references.prds` | list | PRD filenames this item relates to |
| `references.docs` | list | Other doc paths relevant to this item |
| `order` | float | An additional field used by the [Q-CTRL](https://github.com/ksetlak/q-ctrl) app (or any other system that assumes tickets are sorted in some particular order) — agents read it for sorting but never assign or modify it. Lower value = higher on the board. `pickup` ranks `READY` tickets by it first, then by `priority`, then by ID. A missing value or `0.0` (the template default) means "not yet ordered", and those tickets come after all ordered ones. |

## Markdown body

Below the frontmatter, each item has three sections:

- **Description** — 1–5 sentences: problem, goal, scope.
- **Acceptance Criteria** — checkable conditions for `AWAITING_OPERATOR`. Must be testable and observable.
- **Notes** — optional free-form context, open questions, or hints.

Use `_template.md` when creating a new item.

## Security tickets

A ticket with `type: security` is split in two, so that vulnerability details never reach the repository or its history. Both files share one ID, taken from the same sequence as every other ticket.

| File | Tracked | Holds |
|------|---------|-------|
| `items/{ID}.md` (the **stub**) | yes | The full frontmatter, the neutral title `Confidential security ticket`, and a one-line note pointing to the private file. **Status lives here and only here.** |
| `private/{ID}.md` (the **private file**) | no, gitignored | `id` in the frontmatter, then the Description, Acceptance Criteria, and Notes sections: where and how the flaw works, impact, evidence, and the suggested fix. |

Stub body:

```markdown
Details are in `backlog/private/{ID}.md`, which is not tracked by git.
If that file is missing in your copy of the repository, skip this ticket.
```

### Rules

- **Numbering.** The next ID is the highest number across `items/`, `archive/`, and `private/`, plus 1. The stub reserves the number for collaborators, so the operator should commit and push it promptly. This narrows the window for a collision but does not close it.
- **Missing private file means skip.** If a `security` stub has no matching private file, the ticket belongs to another copy of the repository. Agents skip it and never work from the stub alone. Pickup enforces this at selection time.
- **Reading the ticket.** Roles that read a `security` ticket take its status, priority, and dependencies from the stub, and its description and acceptance criteria from the private file.
- **Neutral wording everywhere that is not gitignored.** This covers branch names (use the ticket ID only), commit messages (`Security fix {ID}`), PR titles and descriptions, code comments, test names, docs, and changelogs. It applies to the stub title too. Name no vulnerability class, component, or attack.
- **No exploit code in tracked files.** A test that demonstrates the flaw, or a known-failure marker for it, stays in the private file until the fix ships. Do not copy the debugger's known-failure test practice here. The fix itself may include a regression test with a neutral name.
- **Closing.** After the fix has shipped and the operator accepts it, the operator may paste the private body into the stub, give it a real title, set `status: DONE`, move it to `archive/`, and delete the private file. Making it public is the operator's decision, and agents never do it.

### Backup and sharing

Ignored files have no git history and are not shared. Back up `backlog/private/` yourself if the tickets matter, and share it with collaborators outside git. A repository copy without it can see the stubs but cannot work them.
