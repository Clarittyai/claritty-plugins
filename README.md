# Claritty plugins for Claude Code

Official Claude Code plugin marketplace for [Claritty](https://app.claritty.ai).

## Install

```
/plugin marketplace add Clarittyai/claritty-plugins
/plugin install claritty@claritty
```

## Plugins

### `claritty` — app scaffolder

Adds the `/claritty:new` slash command:

```
/claritty:new my-task-app
```

It scaffolds a new Claritty agentic app by running
[`create-claritty-app`](https://www.npmjs.com/package/create-claritty-app),
then hands off: you `cd` into the new directory and open a fresh session, where
the seed's own `CLAUDE.md` / `.cursorrules` guide app design.

The plugin is a thin wrapper around the CLI, so Codex/Cursor users get the same
result with `npx create-claritty-app my-task-app`.

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
