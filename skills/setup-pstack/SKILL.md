---
name: setup-pstack
description: Configure which models pstack uses per role. Reads each skill's references/models.md, shows the current choices, asks the user, and writes back a kept, idempotent file. Use for /setup-pstack, "configure pstack models", or changing pstack's model choices.
---

# Setup pstack

Write each pstack skill's `references/models.md`, the per-skill override file that sets pstack's model per role. Every skill that reads role→model config has one. The skills read their own file and fall back to the parent model when a line is absent, so this is an override layer, not a requirement.

## Steps

### 1. Locate the per-skill config files

Find pstack's skills wherever they are installed: this repo's `skills/`, `~/.prime/agent/skills/`, or `node_modules/<pkg>/skills/`. Every skill with a `references/models.md` is configurable. Read each file; its header comment documents the role lines and their legal values for that skill.

### 2. Detect available models

Resolve the model selectors you can pass to a child in this session with `await rlm.find_models(...)`; that is the dependable source. If you cannot detect any, ask the user to paste the selectors they have access to. Never write a real selector you have not confirmed is available. The alias `inherit-parent` is always valid even though it is not a detected selector.

### 3. Load current state

For each skill, the current choices are the role lines present in its `references/models.md`. A file with no lines (or no file) means every role falls back to the parent model.

### 4. Map and confirm

Show every role with its current model, marking any real selector not in the detected set as needing a choice. Ask the user whether to accept as-is or change specific roles, offering the detected models plus `inherit-parent` (this role runs on the parent chat model) as the options. For panel roles (arena runners, how critics, architect runners, interrogate reviewers) the value is a list, and one subagent runs per entry, `inherit-parent` entries included, so the list length sets the count. `arena cross-judge pool` is also a list, but Arena selects one value from it whose model family differs from the parent's when possible. `swarm workers` is the default model for every worker unless a race or comparison assigns another model per arm. Follow each skill's own file for its exact role names and list semantics.

### 5. Validate

Every real selector written must be in the detected set; `inherit-parent` always passes. If a chosen real selector is not available, stop and ask again. A file pointing at a model the user cannot use breaks every delegation that reads it.

### 6. Write each file

Write each `references/models.md` with one line per role, using the same labels the skill's own file documents. Keep the file's header and example comments as-is; overwrite the values so re-runs stay idempotent. The reflect file shows the canonical shape:

    # reflect model configuration

    One line per role. Delete a line (or this file) to inherit the parent model.

    # judgment: <model selector>
    # tooling: <model selector>

Resolve a valid selector with `await rlm.find_models(...)` and pass the exact returned selector as `model=` when spawning. `inherit-parent` or omitting `model` runs the child on the parent chat model.

### 7. Confirm

Tell the user the files were written and that the choices apply to new runs. Re-running this skill updates them.

### 8. Offer a verification skill (optional)

Check whether the project has a way to drive the real app for proof (a `verify-*` skill, or an existing harness). If not, offer once: "want a project-local verification skill, so agents can drive the app the way a user does and prove changes work? I can generate one with /create-verification-skill." On yes, invoke `/create-verification-skill`. On no, move on without pushing.
