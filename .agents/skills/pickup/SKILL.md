---
name: pickup
description: >-
  Start or resume agent-driven work on a backlog ticket. Use when the operator
  asks to pick up, execute, or work the next READY item, or to continue
  an in-progress ticket.
---

# Pickup

Entry point for backlog delivery. Runs one ticket end-to-end until `AWAITING_OPERATOR`.

## Invocation

| Command | Behaviour |
|---------|-----------|
| `pickup` | Auto-select the highest-priority `READY` item with satisfied dependencies |
| `pickup {TICKET_ID}` | Work that specific ticket if `READY` (or report why not) |
| `pickup --continue {TICKET_ID}` | Resume an `IN_PROGRESS` or paused ticket from existing run artifacts |

How the skill is triggered depends on your tool: a slash command, a skill picker, or plain language ("pick up the next ticket", "continue APP-001"). The behaviour is identical. Arguments above are positional text, not tool-specific syntax.

## Rules

- **Work until done.** Keep going until the ticket reaches `AWAITING_OPERATOR` or you are genuinely stuck and cannot proceed without operator input.
- **One ticket per invocation.** Do not pick up a second ticket in the same session. Stop cleanly once the current ticket is complete.
- **Never set `DONE` or `CANCELED`.** These are operator-only actions. Agents set `IN_PROGRESS` and `AWAITING_OPERATOR` only.
- **Never pick `OPEN` or `ON-HOLD` items.** Only `READY` items are eligible unless the operator explicitly overrides.
- **Escalate, don't guess.** If a decision requires operator input — architecture, scope, budget — pause, surface the question clearly, and wait. Do not make consequential assumptions and continue.
- **One thing at a time.** Do not start parallel work streams unless the orchestrator has explicitly planned them and the ticket scope justifies it.

## Selecting the next item (when no id given)

1. Read all files in `backlog/items/`.
2. Exclude anything that is not `status: READY`.
3. Exclude items whose `dependencies` are not yet `AWAITING_OPERATOR` or `DONE`.
4. Sort remaining items by `priority`: `must` → `should` → `could`.
5. Among equal priority, take the lowest-numbered ID.
6. If nothing is eligible, report why and stop — do not pick an `OPEN` or `ON-HOLD` item.

## Further reference

Ticket schema, status definitions, and frontmatter fields are in `backlog/README.md`.
