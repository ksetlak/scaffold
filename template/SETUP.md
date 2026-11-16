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

## 3. Configure Scaffold

Open `scaffold.yaml` at the repository root. It's the one place for Scaffold's own settings, and it's meant to be edited.

**`project_key`** is the prefix for ticket IDs (for example `RC` gives `RC-001`, `RC-002`). Set it in `scaffold.yaml`, which is the source of truth. As a best practice, also update the `id` in `backlog/_template.md` to use the same key (for example `RC-000`). Agents read the key from `scaffold.yaml`, but the template is what people copy when they create a ticket by hand, so keeping the two in sync avoids tickets with the wrong prefix.

**`ticket_id_digits`** is how many digits the number in a ticket ID is zero-padded to. The default is 3 (`RC-001`). Set it to 4 or 5 if you expect a large backlog (`RC-0001`). It must be a positive whole number, and a missing or invalid value means 3. If you change it, also update the width of the number in the template's `id` (for example `RC-0000`). Existing tickets keep their names, and IDs are still never reused.

**`subagents`** declares whether your AI tool can run a subagent: an agent with its own fresh context window. Subagents aren't weighed down by other agents' reasoning, and aren't influenced by their approach. That isolation is what makes the reviewer and tester independent, so Scaffold treats subagents as the default way of working.

| Value | Use it when | What happens |
|-------|-------------|--------------|
| `true` (default) | Your tool can spawn agents with a clean context, such as a built-in subagent feature or agent profiles | The orchestrator runs the whole pipeline and delegates each role to its own subagent, in parallel where that helps |
| `false` | Your tool has no such feature, or you can't confirm it isolates context | The orchestrator does its own step, then stops and gives you a ready-to-paste prompt to start the next role in a new session. Roles never share a session |

Scaffold is built to be universal and harness-agnostic, but telling it a little about your stack pays off. It reads this file instead of probing your environment at the start of every run, which keeps sessions cheap. It can still find its way if the value is wrong:

- If it's `true` but no isolated subagent can be spawned, the orchestrator falls back to the manual relay, tells you why, and suggests changing the setting.
- If it's `false` but the tool clearly offers subagents, the orchestrator keeps following the setting and suggests that you change it.

Getting this right lets you make much better use of the tooling you already have. Check it once, and revisit it if you change tools.

## 4. Connect your AI tool

Scaffold is tool-neutral. `AGENTS.md` holds the agent instructions, and `.agents/` holds the role, skill, and mode definitions. Neither is tied to a specific tool, model, or harness. Most tools find them directly. If yours doesn't, add a thin shim that points at the canonical files. Don't copy their content into tool-specific folders.

| Situation | What to do |
|-----------|-----------|
| Tool reads `AGENTS.md` natively | Nothing |
| Tool expects its own instructions file | Create a one-line file that imports or links to `AGENTS.md`, or symlink it. Check your tool's docs for the file name and import syntax |
| Tool expects skills or agents in its own folder | Symlink that folder to `.agents/skills/` or `.agents/agents/`. Don't move the originals |
| Custom or embedded harness | Load `AGENTS.md` as project context, and expose `.agents/skills/` and `.agents/agents/` as skills and roles |
| Tool has no skill support | Ask the agent to read `.agents/skills/pickup/SKILL.md` and follow it |

Role definitions carry no model setting. Choose the model in your tool.

If your tool needs local shim files, either commit them or add them to `.gitignore`, depending on your team's preference.

## 5. Adapt AGENTS.md to your project

Think of `AGENTS.md` as Scaffold's firmware. It's the layer that makes the template work, and it changes rarely. It can be edited at any time, but in practice you'll set it up once for your project and then leave it alone.

Open it and make it yours:

- Replace the placeholder under **Key project rules** with the rules that apply to your project: naming conventions, forbidden libraries or patterns, required file locations, testing requirements, and language and framework constraints.
- Adjust the working style and any other section that doesn't fit how your team works.

Keep the project-specific rules in this file, and keep Scaffold's settings in `scaffold.yaml`. If you later pull newer Scaffold files into your project, this separation keeps the merge small.

## 6. Write your first ADR (optional but recommended)

If you have already made a significant architecture decision (language, framework, database), record it now before you forget the reasoning. Use the template at `docs/adrs/_template.md`.

## 7. Write your first backlog items

Copy `backlog/_template.md` to `backlog/items/{PROJECT_KEY}-001.md`, using the key and the number width (`ticket_id_digits`) from `scaffold.yaml`, for each item. See `backlog/README.md` for the full schema. Set `status: READY` on items you want agents to pick up.

## You're ready

Ask your agent to run the `pickup` skill to start the first backlog item.
