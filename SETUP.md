# Setup

Follow this checklist after cloning Scaffold for a new project.

## 1. Reinitialise git

```bash
rm -rf .git
git init
git add .
git commit -m "chore: init from Scaffold"
```

## 2. Rename the project

- Rename the folder to your project name.
- Find and replace any remaining `scaffold` references in `README.md` and `AGENTS.md`.

## 3. Set the backlog project key

Open `backlog/_template.md` and `backlog/README.md` and replace `APP` with your project key abbreviation (e.g. `RC` for Rovicky-Cedar). Ticket IDs will follow the pattern `RC-001`, `RC-002`, etc.

## 4. Update `.gitignore`

Add this entry for things that should never be committed in a project built on Scaffold:

```
# Ephemeral agent run artifacts
backlog/_runs/
```

If your tool needs local shim files (see step 5), either commit them or ignore them here, depending on your team's preference.

## 5. Connect your AI tool

Scaffold is tool-neutral. `AGENTS.md` holds the project rules, and `.agents/` holds the role and skill definitions. Neither is tied to a specific tool, model, or harness. Most tools find them directly. If yours doesn't, add a thin shim that points at the canonical files. Don't copy their content into tool-specific folders.

| Situation | What to do |
|-----------|-----------|
| Tool reads `AGENTS.md` natively | Nothing |
| Tool expects its own instructions file | Create a one-line file that imports or links to `AGENTS.md`, or symlink it. Check your tool's docs for the file name and import syntax |
| Tool expects skills or agents in its own folder | Symlink that folder to `.agents/skills/` or `.agents/agents/`. Don't move the originals |
| Custom or embedded harness | Load `AGENTS.md` as project context, and expose `.agents/skills/` and `.agents/agents/` as skills and roles |
| Tool has no skill or subagent support | Ask the agent to read `.agents/skills/pickup/SKILL.md` and follow it. The orchestrator runs the roles sequentially |

Role definitions carry no model setting. Choose the model in your tool.

## 6. Add project-specific rules to AGENTS.md

Open `AGENTS.md` and replace the placeholder `_No project-specific rules yet_` section with the rules that apply to your project. Typical entries:

- Naming conventions
- Forbidden libraries or patterns  
- Required file locations
- Testing requirements
- Language and framework constraints

## 7. Write your first ADR (optional but recommended)

If you have already made a significant architecture decision (language, framework, database), record it now before you forget the reasoning. Use the template at `docs/adrs/_template.md`.

## 8. Write your first backlog items

Copy `backlog/_template.md` to `backlog/items/APP-001.md` (replacing `APP` with your project key) for each item. See `backlog/README.md` for the full schema. Set `status: READY` on items you want agents to pick up.

## You're ready

Ask your agent to run the `pickup` skill to start the first backlog item.
