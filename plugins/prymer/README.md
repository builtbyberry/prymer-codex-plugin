# Prymer — Codex plugin

Connects this project to Prymer, the shared context bus, and teaches Codex the ritual: load context at the start of a task, checkpoint at the end.

## What is inside

- `.mcp.json` — connects the hosted Prymer MCP server at `https://swarmplatform.cloud/mcp/prymer` (browser sign-in on install; no token to copy).
- `skills/prymer/` — the Prymer skill: how to use the tools well (loading, checkpointing, curation, handoffs, and dispatch).
- `skills/` — the `prymer-load`, `prymer-checkpoint`, `prymer-onboard`, `prymer-curate`, `prymer-handoff`, `prymer-dispatch`, and `prymer-project` skills (invoke with `$prymer-load` or `/skills`).
- `hooks/hooks.json` — reminders to load context at session start (and again after a compaction) and checkpoint when you finish.

## Install

Downloaded this as a zip? It is a self-contained marketplace — from the directory you unzipped it into:

```bash
codex plugin marketplace add ./
codex plugin add prymer
```

On install, Codex opens Prymer in your browser to authorize. If the connection needs a manual sign-in, run:

```bash
codex mcp login prymer
```

Codex will ask you to trust the plugin hooks on first run (`/hooks`).

## After install

- **The MCP connection** registers as `prymer` when the plugin loads, so the `codex mcp login prymer` above works without adding the server by hand.
- **Context loading is a nudge, not a guarantee.** The `SessionStart` hook only reminds the model to call `get_context_for_injection`; installing the plugin does not by itself load context. The model loads it when it acts on the reminder, or when you ask. For a deterministic load, see Power mode below.
- **Pick the channel.** This bundle does not hard-map this repo to a Prymer channel, so set the active channel each session — tell the model which channel to use, or download a channel-pinned copy from the Connect page to bake one in. The model should not pick a channel on its own from `list_channels`. Run `$prymer-onboard` (or open `/skills`) to bind this project to a channel — it writes a `<!-- prymer-channel: <workspace-slug>/<channel-key> -->` marker to `AGENTS.md` so future sessions know which one to use.

## Power mode (optional)

The hooks above are reminders — they ask the model to load context. For a deterministic load that runs even if the model forgets, install the `prymer` CLI, create a hook credential under Settings → Connected tools, and let the `SessionStart` hook run it:

```bash
npm install -g @builtbyberry/prymer-cli
prymer login   # paste your Prymer URL + the hook token
prymer install-hooks --client codex
```

The hook token lives in the same place it was created — revoke it any time under Settings → Connected tools.

## License

This plugin is licensed under Apache-2.0 (see `LICENSE`) — fork and adapt the connector freely. The hosted Prymer service it connects to is governed by its own terms; "Prymer" is a trademark and the licence grants no rights to it.
