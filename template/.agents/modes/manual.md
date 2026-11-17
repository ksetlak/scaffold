# Manual mode

Used when `subagents` is `false` in `scaffold.yaml` or `scaffold.local.yaml`. The tool can't give a role a fresh context, so the operator starts each role in a new session and you hand off between them. Be strict. Roles never share a session, because the reviewer and tester are only independent if they haven't watched the builder work.

## Run state

Nothing carries over between sessions except what's on disk, so keep `backlog/_runs/{TICKET_ID}/run.json` complete and update it after every step:

- `base_branch`, `base_sha` (optional), `started_at`
- `mode` — `manual`
- `phase` — current phase name
- `next_role` — the role that should run next, or `operator`
- `completed` — finished phases, each with its artifact
- `review_rounds` — builder↔reviewer rounds so far

## Procedure

1. Read `run.json` and the latest artifact. On a fresh run, create `run.json` first.
2. Do only your own step: select the ticket, write the brief, route, or set status.
3. Update `run.json`.
4. Stop. Print the relay prompt for the next role, filled in, and tell the operator to paste it into a **new** session in any tool.

Run one role at a time. Don't plan parallel streams or use worktrees. Parallel work needs subagents.

## Relay prompt

````text
Act as the `{role}` role defined in `.agents/agents/{role}.md`.
Ticket: {TICKET_ID}
Run folder: backlog/_runs/{TICKET_ID}/

You are running in your own session, separate from every other role. Read only the inputs listed in your role definition, and nothing else in the run folder. Write the report your role definition names.

Finish with this block:

Return to orchestrator
Role: {role}
Ticket: {TICKET_ID}
Artifact: backlog/_runs/{TICKET_ID}/{file}
Status: done | blocked | escalation
Next: start the orchestrator again with `pickup --continue {TICKET_ID}`
````

## Coming back

The operator restarts the orchestrator with `pickup --continue {TICKET_ID}`. Read `run.json` and the new artifact, route using the table in `.agents/agents/orchestrator.md`, update `run.json`, and print the next relay prompt.
