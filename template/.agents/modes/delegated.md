# Delegated mode

Used when `subagents` is `true` in `scaffold.yaml` or `scaffold.local.yaml`. Your tool can run a subagent with a fresh context, so use one for every role and spend as little as possible on ceremony.

## Spawning a role

Spawn each role with its definition in `.agents/agents/{role}.md` as instructions. Give it only:

1. The role name
2. The ticket ID
3. The run folder: `backlog/_runs/{TICKET_ID}/`
4. The file name to write, when it differs from the default (a slice or a review lens)
5. This closing instruction: "Reply in at most 10 lines: status (`done`, `blocked`, or `escalation`), the artifact path, and anything the orchestrator must act on."

Never pass your own summaries, opinions, or reasoning. The role reads its inputs from disk.

## Working from replies

Act on the subagent's reply. Open a report only when the reply flags something or you need the detail to route. Don't re-read reports to confirm what the reply already said.

## State

The artifacts on disk are the record of progress: a phase is done when its artifact exists. Don't update `run.json` after every step. Write it:

- At the start: `base_branch`, `base_sha` (optional), `mode: delegated`, `started_at`
- At a gate, with `phase`, `review_rounds`, and `next_role`: an escalation, the loop cap, a failure you can't resolve, or `AWAITING_OPERATOR`
- Whenever you plan parallel work: slices, path ownership, and branches

On `--continue`, rebuild the state from `run.json` and the artifacts that exist.

## Keep going

Move through the phases and the builder↔reviewer and tester→builder loops without asking the operator. Stop only at an escalation, the loop cap, a blocker you can't resolve, or `AWAITING_OPERATOR`.

## Fan out when it pays off

Use parallel subagents when independent work is large enough to outweigh the cost of spawning them. For a small ticket, run one role at a time.

- **Parallel builders.** When the scout's slices have non-overlapping paths, record the slices, path ownership, and branches in `run.json`. Give each builder its own `git worktree` and slice, and have it write `build-report-{slice}.md`. Spawn them together, then run **merger**, then review the integrated result.
- **Parallel reviewers.** For a non-trivial diff, spawn several reviewers at once, each with a distinct lens. Examples are correctness against acceptance criteria, conformance to ADRs and project rules, and edge cases and security. Each writes `review-report-{lens}.md`. Treat the combined findings as one review round.
- **Scout.** For a ticket that spans unrelated areas, scouts can map the areas in parallel. Merge their touch maps into one scout report before the build.

Clean up worktrees before `AWAITING_OPERATOR`.
