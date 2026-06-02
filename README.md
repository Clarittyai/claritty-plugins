# Claritty plugins for Claude Code

Official Claude Code plugin marketplace for [Claritty](https://app.claritty.ai).

## Install

```
/plugin marketplace add Clarittyai/claritty-plugins
/plugin install claritty@claritty
```

## Plugins

### `claritty` — guided app builder

Adds the `/claritty:new` slash command. Just describe the app you want:

```
/claritty:new track which of my plants need watering and remind me
```

Claritty then drives the whole thing in one guided session:

1. **Asks** a few focused follow-ups about your problem, automation, cadence, the
   widget glance, and the look you want.
2. **Scaffolds** a fresh app on your **Desktop** via
   [`create-claritty-app`](https://www.npmjs.com/package/create-claritty-app).
3. **Plans** the app — the `@agent`/`@workflow`/`@trigger` that solve your problem,
   the dashboard widgets + their actions, and a distinct visual identity — and waits
   for your OK.
4. **Builds** it: backend logic + a beautiful app and widgets, following the seed's
   own design/identity/widget rules.
5. **Verifies** with the seed's identity gate so the result never just looks like the
   template, then tells you how to run it.

The agentic seed provides the base structure (the `@agent`/`@workflow` server + UI
kit); the plugin is the conductor.

**Just scaffold (no build):**

```
/claritty:new my-task-app --scaffold-only
```

This runs `create-claritty-app` and hands off — you `cd` into the new directory and
open a fresh session, where the seed's own `CLAUDE.md` / `.cursorrules` guide design.
Codex/Cursor users get the same scaffold with `npx create-claritty-app my-task-app`.

## Layout

```
claritty-plugins/
├── .claude-plugin/
│   └── marketplace.json      # catalog
└── plugins/
    └── claritty/
        ├── .claude-plugin/
        │   └── plugin.json
        └── commands/
            └── new.md         # /claritty:new
```

> This directory is mastered in the `claritty-core` monorepo and published to the
> public `Clarittyai/claritty-plugins` repo that the marketplace command points at.
