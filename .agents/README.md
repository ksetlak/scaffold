# Agents and skills

Tool-neutral definitions for the roles and workflows used in this repository. Nothing here depends on a specific AI tool, model, or harness.

## Layout

```
.agents/
├── agents/   # one Markdown file per role (orchestrator, scout, builder, reviewer, tester, …)
└── skills/   # one folder per skill, each with a SKILL.md (e.g. pickup)
```

## Conventions

- **Plain Markdown.** Each definition is a Markdown file with minimal frontmatter: `name` and `description`. Tools that need extra fields can add them in a local shim, not here.
- **No model pinning.** Definitions never name a model, provider, or reasoning level. The operator chooses these in their tool.
- **No tool-specific syntax.** No slash commands, tool names, MCP server names, or paths such as `.cursor/` or `.claude/`. Refer to skills and roles by name.
- **Project rules live in `AGENTS.md`.** Role definitions point there instead of at tool-specific rule files.
- **Artifacts are the contract.** Roles communicate through files in `backlog/_runs/{TICKET_ID}/`, so the pipeline works whether roles run as separate subagents or sequentially in one session.

## Roles

| Role | Purpose |
|------|---------|
| `orchestrator` | Runs one ticket through the phases; owns status transitions |
| `scout` | Read-only reconnaissance before implementation |
| `architect` | Escalation-only second opinion on architecture |
| `builder` | Implements the ticket |
| `merger` | Integrates parallel branches or worktrees |
| `reviewer` | Reviews the implementation |
| `tester` | Runs and adds tests |
| `technical-writer` | Updates documentation |
| `adr-expert` | Drafts and maintains ADRs |
| `prd-expert` | Drafts and maintains PRDs |

## Skills

| Skill | Purpose |
|-------|---------|
| `pickup` | Select and deliver a backlog ticket until `AWAITING_OPERATOR` |

See `SETUP.md` for making your tool discover these files.
