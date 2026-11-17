# Agent Guide

This is the canonical agent instruction file for this repository. All AI tools should read this file. If your tool uses a different entry point, point it here. See `SETUP.md` for how.

## Repository orientation

- `AGENTS.md` — this file; rules and working guidance for AI agents
- `README.md` — project overview, repository layout, developer setup
- `backlog/README.md` — backlog schema, statuses, agent workflow
- `backlog/items/` — one file per open backlog item; `backlog/archive/` for ones closed and archived
- `docs/adrs/` — Architecture Decision Records (index: `docs/adrs/README.md`)
- `docs/prds/` — Product Requirements Documents (index: `docs/prds/README.md`)
- `.agents/` — role-specific agent definitions, skills, and execution modes (index: `.agents/README.md`)
- `.copier-answers.yml` — project properties chosen at creation (ticket ID prefix and number width). Managed by Copier; never edit by hand
- `scaffold.yaml` — Scaffold settings that can change during the project (subagent support). `scaffold.local.yaml`, if present, holds one operator's personal overrides and is not committed

When exploring a documentation subfolder, read its `README.md` as an index before opening individual files.

## Working style

- Make small, reviewable changes. Avoid broad refactors unless they directly unblock the current task.
- When requirements are unclear, stop and ask the operator. Do not make assumptions on product or architecture decisions.
- Record all significant product and architecture decisions as PRDs or ADRs in `docs/`.
- Update `README.md` and relevant docs when adding commands, services, environment variables, or changing project structure.

## Backlog delivery

Use the `pickup` skill (`.agents/skills/pickup/`) to work on backlog items. See `backlog/README.md` for the full workflow, ticket schema, and status rules.

## Configuration

Scaffold's configuration lives in these files. Read them when a task depends on them. None of them is project documentation.

- `.copier-answers.yml` holds the ticket ID prefix (`project_key`) and number width (`ticket_id_digits`). Never edit it by hand. Copier manages it.
- `scaffold.yaml` holds settings the operator may change at any time, such as whether subagents are available. It is committed and holds the project's shared defaults.
- `scaffold.local.yaml` is optional and not committed. It holds one operator's personal overrides. Read it after `scaffold.yaml`, and let its keys win. Never create it or commit it on the operator's behalf.

## Dependency checks

Before adding any new dependency, verify it is appropriate for this project's constraints and document the decision if it represents an architectural shift.

## Key project rules

_No project-specific rules yet. Replace this line with your own after cloning. `SETUP.md` step 5 lists what belongs here: naming conventions, forbidden libraries or patterns, required file locations, testing requirements, and language and framework constraints._
