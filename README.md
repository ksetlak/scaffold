# Scaffold

"The Mothership has cleared the Scaffold. We are away."

An opinionated, tool-agnostic project template for AI-assisted software development. It works with any agent harness that can read Markdown, such as Cursor, Claude Code, or Spotify Studio. Clone it, remove `.git`, rename the folder, and you have a ready-made factory for building a new application with AI agents doing the heavy lifting.

## What's included

| Path | Purpose |
|------|---------|
| `.agents/` | Role-specific agent definitions (orchestrator, builder, reviewer, tester, and more) and the `pickup` skill for backlog delivery |
| `backlog/` | Structured backlog — one Markdown file per item, with YAML frontmatter, schema rules, and agent workflow reference |
| `docs/adrs/` | ADR template and index — record every significant architecture decision |
| `docs/prds/` | PRD template and index — capture product requirements before implementation |
| `docs/meeting-notes/` | Running log of decisions made with the operator |
| `AGENTS.md` | Canonical, tool-neutral AI agent instructions |
| `SETUP.md` | Checklist for configuring a fresh clone for your specific project and tools |

## Where to look

- **How agents work**: `.agents/README.md`
- **Backlog rules and ticket schema**: `backlog/README.md`
- **ADR template**: `docs/adrs/_template.md`
- **PRD template**: `docs/prds/_template.md`
- **AI tool setup**: `SETUP.md`

## Repository layout

```
scaffold/
├── .agents/
│   ├── README.md        # Conventions and role index
│   ├── agents/          # Role definitions: orchestrator, builder, reviewer, …
│   └── skills/pickup/   # pickup skill for backlog-driven delivery
├── backlog/
│   ├── items/           # one .md file per open item
│   ├── archive/         # one .md file per closed item
│   ├── _template.md     # copy when creating a new item
│   └── README.md # Schema, statuses, workflow rules
├── docs/
│   ├── adrs/            # Architecture Decision Records
│   ├── prds/            # Product Requirements Documents
│   └── meeting-notes/   # Session logs
├── AGENTS.md            # Canonical AI agent instructions
├── README.md            # This file
└── SETUP.md             # Per-project and per-tool setup checklist
```

## Getting started

```bash
git clone <this-repo> my-project
cd my-project
rm -rf .git
git init
```

Then follow `SETUP.md`.
