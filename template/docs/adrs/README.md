# Architecture Decision Records

An ADR captures a significant architectural choice: what was decided, why, what alternatives were considered, and what consequences follow. The goal is not bureaucracy — it is making sure future contributors (human or AI) can understand *why* the codebase looks the way it does, and can challenge or revisit decisions with full context.

## When to write an ADR

Write one when a decision:

- is hard to reverse or expensive to change later,
- involves real trade-offs between alternatives (not just "the obvious choice"),
- would otherwise be invisible — buried in a PR, a chat message, or someone's memory.

You do not need an ADR for every library added or every naming convention. If you find yourself writing "we chose this because it was the default", it probably does not need an ADR.

## Status lifecycle

| Status | Meaning |
|--------|---------|
| `DRAFT` | Being written or discussed — not yet binding |
| `ACCEPTED` | In force; the codebase should reflect it |
| `DEPRECATED` | No longer recommended; no direct replacement |
| `SUPERSEDED by ADR-NNNN` | Replaced by a newer decision; follow the newer one |

Leave superseded ADRs in place — they are the historical record. Update their status and link the replacement.

## How to create a new ADR

1. Copy `_template.md` to a new file named `NNNN-short-slug.md` (e.g. `0004-database-choice.md`).
2. Use the next available number in sequence. Numbers are never reused.
3. Fill in all sections. Be honest about trade-offs — an ADR that only argues for the chosen option is not useful.
4. Set status to `DRAFT` until the decision is ratified, then `ACCEPTED`.
5. Add a row to the index table below.

## Index

| ADR | Title | Status |
|-----|-------|--------|
| — | — | — |
