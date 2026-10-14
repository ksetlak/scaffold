# Agents and skills

Tool-neutral definitions for the roles and workflows used in this repository. Nothing here depends on a specific AI tool, model, or harness.

## Layout

```
.agents/
├── agents/   # one Markdown file per role (orchestrator, scout, builder, reviewer, tester, …)
├── modes/    # how roles are run: delegated.md (subagents) and manual.md (relay)
└── skills/   # one folder per skill, each with a SKILL.md (e.g. pickup)
```

## Conventions

- **Plain Markdown.** Each definition is a Markdown file with minimal frontmatter: `name` and `description`. Tools that need extra fields can add them in a local shim, not here.
- **No model pinning.** Definitions never name a model, provider, or reasoning level. The operator chooses these in their tool.
- **No tool-specific syntax.** No slash commands, tool names, MCP server names, or paths such as `.cursor/` or `.claude/`. Refer to skills and roles by name.
- **Project rules live in `AGENTS.md`.** Role definitions point there instead of at tool-specific rule files.
- **Roles are isolated.** Each role runs in its own fresh session, so reviewer and tester stay independent of the builder. The orchestrator is the hub that routes between them. Role files describe the work; the mode files describe how roles are started and handed off.
- **Stack is declared, not probed.** Whether subagents are available is set in `scaffold.yaml`, and the orchestrator loads one mode file to match.

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
| `refactorer` | Specialist. Improves code structure without changing behavior |

Specialists have one job each and run on their own, outside the default pipeline. Start one in a fresh session with a prompt such as: "Act as the `debugger` role defined in `.agents/agents/debugger.md`. Target: `{ticket ID, path, or 'free roam'}`."

## Skills

| Skill | Purpose |
|-------|---------|
| `pickup` | Select and deliver a backlog ticket until `AWAITING_OPERATOR` |

Only the mode that matches `scaffold.yaml` is loaded in a session. See `SETUP.md` for making your tool discover these files.
