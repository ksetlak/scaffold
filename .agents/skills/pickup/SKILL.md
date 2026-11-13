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
| `pickup --continue {TICKET_ID}` | Resume an `IN_PROGRESS` or paused ticket from `run.json` and the existing run artifacts. Also the way back to the orchestrator after a manually relayed role finishes |

How the skill is triggered depends on your tool: a slash command, a skill picker, or plain language ("pick up the next ticket", "continue APP-001"). The behaviour is identical. Arguments above are positional text, not tool-specific syntax.

## Rules

- **Work until done, or until the next hand-off.** Follow the execution mode set in `scaffold.yaml` (see `.agents/agents/orchestrator.md`). In delegated mode, keep going until `AWAITING_OPERATOR` or an operator gate. In manual mode, complete one step and stop with the relay prompt for the next role.
- **One ticket per invocation.** Do not pick up a second ticket in the same session. Stop cleanly once the current ticket is complete.
- **Never set `DONE` or `CANCELED`.** These are operator-only actions. Agents set `IN_PROGRESS` and `AWAITING_OPERATOR` only.
- **Never pick `OPEN` or `ON-HOLD` items.** Only `READY` items are eligible unless the operator explicitly overrides.
- **Escalate, don't guess.** If a decision requires operator input — architecture, scope, budget — pause, surface the question clearly, and wait. Do not make consequential assumptions and continue.
- **Parallelism is the orchestrator's call.** Parallel work streams are only for delegated mode, when the ticket scope justifies them.

## Selecting the next item (when no id given)

1. Read all files in `backlog/items/`.
2. Exclude anything that is not `status: READY`.
3. Exclude items whose `dependencies` are not yet `AWAITING_OPERATOR` or `DONE`.
4. Exclude `type: security` items whose private file `backlog/private/{ID}.md` does not exist (see below).
5. Sort remaining items by `priority`: `must` → `should` → `could`.
6. Among equal priority, take the lowest-numbered ID.
7. If nothing is eligible, report why and stop — do not pick an `OPEN` or `ON-HOLD` item.

### Report the selection

Before any work starts, print a short selection log. This is a report, not a gate: do not ask for confirmation, and continue straight into the work.

- **Picked:** the ticket ID and title, with the reason in one line (for example, "highest priority `must`, lowest ID among equals").
- **Skipped:** every ticket that would have ranked ahead of the picked one but was excluded, each with a one-line reason: not `READY`, a dependency that isn't `AWAITING_OPERATOR` or `DONE` yet (name it), or a missing private file for a `security` ticket.
- If more than 5 tickets were skipped, list the 5 that would have ranked highest and give the count of the rest.

When the operator names a ticket with `pickup {TICKET_ID}`, there is no selection to explain, so skip the log.

## Security tickets

A ticket with `type: security` is a public stub. Its real content is in `backlog/private/{ID}.md`, which is gitignored (see "Security tickets" in `backlog/README.md`).

- **No private file, no work.** If the file is missing, the ticket belongs to another copy of the repository. Skip it when selecting. When the operator names it with `pickup {TICKET_ID}`, report that the private file is missing and stop. Never work from the stub alone, and never guess its content from the ID or title.
- **Start the orchestrator with the private file in hand.** The orchestrator takes the description and acceptance criteria from the private file.
- **Neutral wording.** Keep names, messages, and anything you print that may be committed free of vulnerability details. Branches use the ticket ID only, and commits say `Security fix {ID}`.

## Further reference

Ticket schema, status definitions, and frontmatter fields are in `backlog/README.md`.
