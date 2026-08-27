# Porting conventions (batch translation table)

You are porting Cursor-plugin skill text to prime-agent. Apply these EXACT replacements. Style reference: `skills/reflect/SKILL.md` and `skills/arena/SKILL.md` (already ported) — match their wording.

## Mandatory translations

| Cursor text | prime-agent text |
|---|---|
| `Task` call / `Task` calls / one message, N `Task` calls | `await rlm(prompt, name=<slug>)` child spawns; "spawn all N with N `await rlm(...)` calls in one turn" |
| `subagent_type: generalPurpose` | delete the line/concept |
| `run_in_background: true` | delete; children run independently |
| agent mode / `readonly: true` / `readonly: false` | delete; prime-agent has no readonly mode; children keep MCP access when configured |
| `model: <cursor slug>` (grok-4.6-fast-xhigh, claude-fable-5-thinking-max, gpt-5.6-sol-max, claude-opus-5-thinking-xhigh...) | `references/models.md` when configured, otherwise omit `model` and inherit the parent model; resolve selectors via `await rlm.find_models(...)` |
| `~/.cursor/rules/pstack-models.mdc` | `references/models.md` (per-skill file; create it if the skill mentions role→model config and it doesn't exist yet) |
| "returns findings in the Task response body" | "replies via `await agent_message.send(message, receiver_role='parent')`" |
| `agent-transcripts/` dir, transcript layouts | `~/.prime/agent/sessions/*.jsonl`; child rollouts in `session_dir` |
| `AskQuestion` | plain text: "ask the user" / "asking the user" |
| Cursor's built-in `create-skill` | `skill-creator` (prime-agent's built-in skill) |
| `cursor-team-kit` skills (`deslop`, `control-cli`, `control-ui`, `babysit`) | remove the hook/bullet entirely (not ported) |
| `.cursor/skills/`, `~/.cursor/skills/`, `~/.cursor/plugins/` paths | `.prime/agent/skills/`, `~/.prime/agent/skills/`, `node_modules/<pkg>/skills/` and built-ins under the prime-agent install |
| "list available MCPs from the Cursor environment / mcps/ directory" | "enumerate the MCP servers configured for this session (prime CLI / MCP Connections); use the tools they expose" |

## Rules

1. Preserve methodology text verbatim. Only translate Cursor-mechanic sentences; if a sentence mixes method + mechanics, keep the method wording and replace only the mechanic clause.
2. Markdown structure, headings, tables, step numbers: keep.
3. Do not invent prime-agent API beyond: `await rlm(task, name=...)`, `await rlm.find_models(...)`, `await agent_message.send(message, receiver_role='parent')`, `~/.prime/agent/sessions/`, `references/models.md`, `skill-creator`, `/reload`. Never write `rlm.get_models` or other invented names.
4. Keep true external-tool references that are environment-agnostic (git, gh, graphite, MCP servers, bun, etc.) unless they are the Cursor plugin mechanism itself.
5. poteto-mode frontmatter: remove Cursor-only fields `mode:`, `icon:`, `color:`, `reminder:`; fold the reminder text into the first body line as a short note.
6. `create-skill` routing in reflect: already done — do not touch reflect/arena files.

## Acceptance check (must be clean on YOUR files)

    grep -rn -i "subagent_type\|run_in_background\|AskQuestion\|~/.cursor\|agent-transcripts\|cursor-team-kit\|Task tool\|readonly\|grok-4.6\|fable-5\|sol-max\|opus-5\|\.cursor/" <your files>

For the word "Cursor" and "Task" (non-mechanic prose): acceptable to leave ONLY brand/history prose like "at Cursor" or "Cursor exposes"; mechanism uses must be translated. If unsure, translate.

## Per-file report

Reply to parent with, per file: line-change count (diff vs upstream at /tmp/cursor-plugins/pstack/skills/) and anything you skipped + why.
