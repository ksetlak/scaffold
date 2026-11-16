# Setup

Follow this checklist after creating a project from Scaffold.

## 1. Check the generated files

Copier asked for your project name, ticket ID prefix, and ticket number width, then generated this project. Your answers are in `.copier-answers.yml`. Copier manages that file and needs it for updates, so never edit it by hand.

If you generated into a new folder, initialise git and commit:

```bash
git init
git add .
git commit -m "chore: init from Scaffold"
```

Commit `.copier-answers.yml` along with everything else. Updates depend on it.

## 2. Add Scaffold to an existing repository

To add Scaffold to a repository that already exists, run Copier from inside it instead of generating a new folder. Start from a clean working tree. Copier asks the same questions, then copies the Scaffold files in. If a file with the same name already exists, Copier asks what to do, so review each one. `AGENTS.md` is the most likely overlap. Keep your existing rules and merge Scaffold's sections around them.

## 3. Understand the two configuration files

| File | Holds | Edit it? |
|------|-------|----------|
| `.copier-answers.yml` | Project properties chosen once: `project_key` (ticket ID prefix) and `ticket_id_digits` (width of the ticket number) | No. Copier manages it |
| `scaffold.yaml` | Settings that can change during the project, currently `subagents` | Yes, whenever your situation changes |

**`project_key`** is the prefix for ticket IDs (for example `RC` gives `RC-001`, `RC-002`). **`ticket_id_digits`** is how many digits the number is zero-padded to. The default is 3 (`RC-001`). Existing tickets keep their names if you change either one later, and IDs are never reused. Both also shape the `id` in `backlog/_template.md`, which is generated from your answers.

To change an answer after creation, run `copier update` and answer the questions again. See "Updating Scaffold" below.

**`subagents`** declares whether your AI tool can run a subagent: an agent with its own fresh context window. Subagents aren't weighed down by other agents' reasoning, and aren't influenced by their approach. That isolation is what makes the reviewer and tester independent, so Scaffold treats subagents as the default way of working.

| Value | Use it when | What happens |
|-------|-------------|--------------|
| `true` (default) | Your tool can spawn agents with a clean context, such as a built-in subagent feature or agent profiles | The orchestrator runs the whole pipeline and delegates each role to its own subagent, in parallel where that helps |
| `false` | Your tool has no such feature, or you can't confirm it isolates context | The orchestrator does its own step, then stops and gives you a ready-to-paste prompt to start the next role in a new session. Roles never share a session |

Scaffold is built to be universal and harness-agnostic, but telling it a little about your stack pays off. It reads this file instead of probing your environment at the start of every run, which keeps sessions cheap. It can still find its way if the value is wrong:

- If it's `true` but no isolated subagent can be spawned, the orchestrator falls back to the manual relay, tells you why, and suggests changing the setting.
- If it's `false` but the tool clearly offers subagents, the orchestrator keeps following the setting and suggests that you change it.

Getting this right lets you make much better use of the tooling you already have. Revisit it whenever you switch tools.

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

Keep the project-specific rules in this file. When you update Scaffold, Copier merges its changes into your edits. It only raises a conflict where you and Scaffold changed the same lines.

## 6. Write your first ADR (optional but recommended)

If you have already made a significant architecture decision (language, framework, database), record it now before you forget the reasoning. Use the template at `docs/adrs/_template.md`.

## 7. Write your first backlog items

Copy `backlog/_template.md` to `backlog/items/{PROJECT_KEY}-001.md`, using the key and the number width from `.copier-answers.yml`, for each item. See `backlog/README.md` for the full schema. Set `status: READY` on items you want agents to pick up.

## You're ready

Ask your agent to run the `pickup` skill to start the first backlog item.

## Updating Scaffold

Scaffold releases are tagged versions. To move your project to the latest release, commit or stash your changes so the working tree is clean, then run `copier update` from the project root. Copier compares the version your project was generated from with the new one and applies only what changed between them, so your edits are kept. Where you and Scaffold changed the same lines, it leaves conflict markers for you to resolve. Review the result and commit it.

`copier update` also lets you change your answers, such as `project_key`. Copier needs `.copier-answers.yml` to know which version you're on, so keep it committed and unedited.
