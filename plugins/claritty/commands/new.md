---
description: Scaffold a new Claritty agentic app from the agentic-app-seed template
argument-hint: <app-name>
allowed-tools: Bash(npx:*)
---

Scaffold a new Claritty app named `$ARGUMENTS` by running the official CLI:

!`npx -y create-claritty-app@latest $ARGUMENTS --yes`

The command above clones the `agentic-app-seed` template, creates a local `.env`,
and initialises a fresh git repository.

Now do exactly this and nothing more:

1. Relay the scaffolder's **Next steps** output to the user verbatim.
2. Stop. Do **not** start editing files, install dependencies, or run a
   brainstorming/design flow in this session.

The user will `cd` into the newly created directory and open a fresh session
there — the seed ships its own `CLAUDE.md`, `.cursorrules`, and `.claude/` config,
which take over and guide app design from inside the project. This command's only
job is the one-command scaffold + hand-off.
