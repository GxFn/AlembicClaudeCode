# Alembic — Claude Code Plugin Shell

This repository is the **Claude Code** distribution shell for the Alembic
plugin. Its sibling, [AlembicCodex](https://github.com/GxFn/AlembicCodex),
is the Codex distribution shell. Each shell owns exactly one host
(per-host shells, user decision 2026-06-12); the actual runtime is the
pinned npm package `@gxfn/alembic-codex-runtime`, bootstrapped by
`bin/alembic-codex-start.mjs`.

## Layout (official Claude Code plugin format)

- `.claude-plugin/plugin.json` — plugin manifest. MCP server is declared
  inline in spec form (`${CLAUDE_PLUGIN_ROOT}` paths) because Claude Code
  copies installed plugins into its cache and spawns MCP servers from the
  session cwd, so relative paths do not resolve.
- `skills/` — five skills (`alembic`, `alembic-create`, `alembic-guard`,
  `alembic-recipes`, `alembic-structure`), auto-discovered at the plugin
  root and namespaced as `alembic-codex:<skill>`.
- `bin/alembic-codex-start.mjs` — runtime bootstrap: ensures the pinned
  npm runtime in a writable cache, then execs the MCP server over stdio.

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
