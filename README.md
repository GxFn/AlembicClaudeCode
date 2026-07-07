# Alembic — Claude Code Plugin Shell

This repository is the **Claude Code** distribution shell for the Alembic
plugin. Its sibling, [AlembicCodex](https://github.com/GxFn/AlembicCodex),
is the Codex distribution shell. Each shell owns exactly one host
(per-host shells, user decision 2026-06-12); the actual runtime is the
pinned npm package `@gxfn/alembic-runtime@0.3.0`, bootstrapped by
`bin/alembic-start.mjs`.

## Layout (official Claude Code plugin format)

- `.claude-plugin/plugin.json` — plugin manifest. MCP server is declared
  inline in spec form (`${CLAUDE_PLUGIN_ROOT}` paths) because Claude Code
  copies installed plugins into its cache and spawns MCP servers from the
  session cwd, so relative paths do not resolve.
- `skills/` — five skills (`alembic`, `alembic-create`, `alembic-guard`,
  `alembic-recipes`, `alembic-structure`), auto-discovered at the plugin
  root and namespaced as `alembic:<skill>`.
- `bin/alembic-start.mjs` — runtime bootstrap: ensures the pinned
  npm runtime in a writable cache, then execs the MCP server over stdio.

## Install from the marketplace

This repo doubles as its own marketplace (`.claude-plugin/marketplace.json`
at the repo root, the spec-default hosting shape; the plugin entry uses the
`github` source type pointing back at this repo):

```bash
claude plugin marketplace add GxFn/AlembicClaudeCode
claude plugin install alembic@gxfn
```

Alternatives considered and not used: a relative `"./"` self-source (works
only for git-cloned marketplace adds and makes the cache copy carry the
marketplace file ambiguity) and a separate catalog repo (an extra repo to
maintain for a single plugin). Revisit when more plugins join the catalog.

### If the MCP server does not connect on first use

Claude Code negative-caches a failed MCP start in
`<config>/mcp-needs-auth-cache.json` and silently skips the server on later
sessions. If the very first connect failed (for example the pinned runtime was
not yet reachable), clear that negative cache once after the cause is fixed —
either run `/mcp` and choose **Reconnect** for the `alembic` server, or delete
`mcp-needs-auth-cache.json` under your Claude Code config directory and start a
new session. The runtime is pinned to an exact version
(`@gxfn/alembic-runtime@0.3.0`); reconnecting reuses the same pin.

## Try it locally

```bash
claude --plugin-dir /path/to/this/repo
```

`claude plugin validate . --strict` passes on this tree.

## Provenance and pending decisions

Initial content is a byte-identical copy of the CC1-verified shell state
(AlembicCodex commit `05802b1`): validate `--strict` PASS, live
scratch-profile load with 5 skills registered, stdio MCP connected, and a
26-tool surface matching the certification matrix. See
`PLUGIN-SOURCE.json` for the cross-shell sync rule and the recorded
host-wording debt (names/keywords/skill bodies still say "Codex";
renaming is owned by later CC2/CC3 waves). The marketplace catalog
(`.claude-plugin/marketplace.json`) is deliberately absent — that
decision belongs to the CC2 distribution sub-wave.
