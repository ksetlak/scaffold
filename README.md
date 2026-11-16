# Scaffold

"The Mothership has cleared the Scaffold. We are away."

An opinionated, tool-agnostic project template for AI-assisted software development. It works with any agent harness that can read Markdown, such as Cursor, Claude Code, or OpenCode. Generate a project from it and you have a ready-made factory for building a new application with AI agents doing the heavy lifting.

Scaffold is distributed as a [Copier](https://copier.readthedocs.io) template. Copier generates the project, and later brings Scaffold's improvements into it without overwriting your work.

## What a generated project contains

| Path | Purpose |
|------|---------|
| `.agents/` | Role-specific agent definitions (orchestrator, builder, reviewer, tester, and more) and the `pickup` skill for backlog delivery |
| `backlog/` | Structured backlog: one Markdown file per item, with YAML frontmatter, schema rules, and agent workflow reference |
| `docs/adrs/` | ADR template and index. Record every significant architecture decision |
| `docs/prds/` | PRD template and index. Capture product requirements before implementation |
| `docs/meeting-notes/` | Notes from meetings related to the project |
| `AGENTS.md` | Canonical, tool-neutral AI agent instructions. Adapt it once to your project |
| `scaffold.yaml` | Settings that can change during the project, such as whether your tool supports subagents |
| `.copier-answers.yml` | Project properties chosen at creation (ticket ID prefix and number width). Managed by Copier |
| `SETUP.md` | Checklist for configuring the new project for your tools |
| `README.md` | A starter README for your project |

## Terminology

The person who owns the project and directs the agents is referred to as the operator. Scaffold's files use this word instead of "user", because "user" usually means the end user of the software being built.

## Repository layout

This repository is the template, not a project. Only the contents of `template/` reach a generated project.

```
scaffold/
├── copier.yml           # Copier configuration and the questions it asks
├── README.md            # This file (not copied into projects)
└── template/            # Everything a generated project receives
    ├── .agents/
    │   ├── README.md        # Conventions and role index
    │   ├── agents/          # Role definitions: orchestrator, builder, reviewer, …
    │   ├── modes/           # Delegated (subagents) and manual (relay) execution modes
    │   └── skills/pickup/   # pickup skill for backlog-driven delivery
    ├── backlog/
    │   ├── items/           # one .md file per open item
    │   ├── archive/         # one .md file per closed item
    │   ├── _template.md.jinja  # becomes backlog/_template.md, with your ticket ID format
    │   └── README.md        # Schema, statuses, workflow rules
    ├── docs/
    │   ├── adrs/            # Architecture Decision Records
    │   ├── prds/            # Product Requirements Documents
    │   └── meeting-notes/   # Session logs
    ├── AGENTS.md            # Canonical AI agent instructions
    ├── scaffold.yaml        # Settings that can change during the project
    ├── SETUP.md             # Per-project and per-tool setup checklist
    ├── README.md.jinja      # Starter README for the generated project
    └── {{ _copier_conf.answers_file }}.jinja  # Records your answers for updates
```

## Requirements

Generating and updating a project needs [Copier](https://copier.readthedocs.io) (Python 3.10 or newer) and Git 2.27 or newer. Using the generated project needs neither: it is plain Markdown and YAML.

Install Copier with `pipx install copier` or `uv tool install copier`, or run it without installing through `uvx copier`.

## Getting started

Create a new project:

```bash
copier copy <this-repo-url> my-project
cd my-project
git init && git add . && git commit -m "chore: init from Scaffold"
```

Copier asks for the project name, the ticket ID prefix, and the ticket number width. Then follow `SETUP.md` in the new project.

To add Scaffold to a repository that already exists, run `copier copy <this-repo-url> .` from inside it, with a clean working tree.

## Updating a project

From the project root, with a clean working tree:

```bash
copier update
```

Copier applies what changed in Scaffold since your project was generated, and keeps your own edits. Where you changed the same lines as Scaffold, it leaves conflict markers for you to resolve. Keep `.copier-answers.yml` committed and never edit it by hand.

Updates work from tagged releases, so a project can only be updated to a version that has a tag.

---

## License

MIT — see [LICENSE](LICENSE).  
Copyright (c) 2026 Krzysztof Setlak.
