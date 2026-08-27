# prime-pstack

[pstack](https://github.com/cursor/plugins/tree/main/pstack) skills ported to
[prime-agent](https://primeintellect.ai). Rigorous agent workflows: fewer,
higher-quality lines, verifiable work, and subagents you can parallelize with
confidence.

This is a **port**, not the original: Cursor-specific mechanics (`Task`
subagents, `~/.cursor/rules`, `AskQuestion`, Cursor model slugs) are translated
to prime-agent equivalents (`rlm` children, `agent_message`, `references/models.md`,
session logs). The methodology text is preserved verbatim wherever it did not
reference Cursor.

## Install

**Via npm (recommended)**

```bash
prime-agent package install npm:prime-pstack                 # all your sessions
prime-agent package install --local npm:prime-pstack         # one project only
```

prime-agent installs the package, registers it under `packages` in
`~/.prime/agent/settings.json` (or `.prime/settings.json` with `--local`), and
loads its 45 skills. Update with `prime-agent package update npm:prime-pstack`,
remove with `prime-agent package remove npm:prime-pstack`.

New skills load on session start; in an interactive session use `/reload`.

> Note: a plain `npm install prime-pstack` into a project's `node_modules` does
> **not** register the package with prime-agent — the `package install` command
> is what adds the `skills/` directory to your settings.

**From the GitHub repo**

```bash
git clone https://github.com/eryzerz/prime-pstack ~/.prime/agent/skills/prime-pstack
```

The loader scans nested skill directories, so cloning the repo into your
skills dir works as-is. Per-project: clone or symlink `skills/` into
`.prime/agent/skills/` at your project root.

## Start here

- `/skill:poteto-mode` — the hub skill; routes a task to one of 23 playbooks,
  wraps code in the principle skills. This is the one to use daily.
- `/skill:setup-pstack` — configure per-role models by editing each skill's
  `references/models.md` (default: children inherit the parent model).
  You can also edit those files by hand — one `role: <model selector>` line
  per role, resolved via `await rlm.find_models(...)`.

## What was ported

All 45 skills (markdown + references + playbooks) plus the `poteto-mode`
helper scripts, with Cursor mechanics translated:

| pstack (Cursor) | prime-agent |
|---|---|
| `Task` calls, `subagent_type`, `run_in_background` | `await rlm(prompt, name=...)` children, spawned in parallel |
| `model: <cursor slug>` defaults | `references/models.md` per skill; omit `model` to inherit parent |
| findings in `Task` response body | `await agent_message.send(..., receiver_role='parent')` |
| `~/.cursor/rules/pstack-models.mdc` | `references/models.md` in each skill that reads model config |
| `agent-transcripts/` | `~/.prime/agent/sessions/*.jsonl` (child rollouts in `session_dir`) |
| MCP discovery via Cursor env | MCP servers configured via prime CLI; children keep MCP access |
| Cursor built-in `create-skill` | prime-agent `skill-creator` |
| `cursor-team-kit` (`deslop`, `control-cli/ui`) | not ported — hooks removed |

## Not ported (deliberately)

- `automations/benny` (cron issue triage → use prime-agent heartbeats)
- `agents/*` (Cursor agent definitions) — except `comment-sicko`, folded into
  `no-comments/references/comment-sicko.md`
- `docs/guide/` images

The `poteto-mode` helper scripts are ported (`scripts/`): `watch-pr` (GitHub
PR watcher, needs `bun`), `orch` (orchestrate store, needs `bun`),
`check-plan.mjs` (plan validator, needs `node`), `worktree-audit.sh` (pure
bash). All are runtime-agnostic aside from their runtimes; `worktree-audit.sh`
and `check-plan.mjs` were translated for prime-agent paths.

## Contributing

`PORTING.md` in this repo is the translation table used for the port. When you
fix or extend a skill, keep the conventions there so the port stays coherent.

## License

MIT. Port of [cursor/plugins pstack](https://github.com/cursor/plugins/tree/main/pstack)
by Lauren Tan, which remains MIT-licensed; see `LICENSE`.
