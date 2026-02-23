## Context

This is a greenfield repo (no commits yet) that will become a central skill management hub, inspired by [PaulRBerg/dot-agents](https://github.com/PaulRBerg/dot-agents). That project uses `just` as its task runner. We're using `mise` instead, which also handles tool version management.

The Vercel `skills` CLI (`npx skills` / `skills`) handles the heavy lifting: fetching skill repos from GitHub, creating symlinks from agent global directories back to canonical copies, and maintaining a `.skill-lock.json`. Our mise tasks wrap around this CLI and add the shelf pattern on top.

Target agents: OpenCode (`~/.config/opencode/skills/`), Codex (`~/.codex/skills/`).

## Goals / Non-Goals

**Goals:**
- Provide a single repo that manages all agent skills, version-controlled with git
- Use mise for both tool management (`npm:skills`) and task running (file-based tasks)
- Support the active/shelf pattern for toggling skills without uninstalling them
- Keep the setup minimal -- no unnecessary tooling or configuration

**Non-Goals:**
- Publishing custom skills to external repos (this repo consumes skills, doesn't publish them)
- Supporting agents beyond OpenCode and Codex (can be added later by editing task scripts)
- CI/CD or GitHub Actions (can be added later if desired)
- The `.opencode/` directory and OpenSpec workflow are project-local tooling for managing this repo; they are gitignored and not part of the deliverable

## Decisions

### Use file-based mise tasks (`.mise/tasks/`) instead of inline `[tasks]` in `mise.toml`

The skill lifecycle tasks (`install`, `reset`) have enough shell logic (loops, conditionals, arrays) that inline TOML strings become awkward to read and edit. File-based tasks are executable scripts with a comment-header for metadata, one file per task.

**Alternative considered:** Inline tasks in `mise.toml`. Rejected because multi-line shell in TOML triple-quoted strings is hard to maintain, and mise's file-based tasks are a first-class feature.

### Use `npm:skills` as a mise-managed tool

Rather than requiring a global Node.js install and using `npx skills`, we declare `npm:skills` in `mise.toml [tools]`. This means `mise install` sets up the skills CLI, and task scripts call `skills` directly (not `npx skills`).

**Alternative considered:** Pinning `node` in mise and using `npx`. Rejected because `npm:skills` is more direct -- mise handles the npm package as a tool, no separate node version pin needed.

### Use `uvx mdformat` for markdown formatting (not a mise-managed tool)

The `mdformat` tool is a Python package. Rather than adding a Python/pip dependency to mise, the formatting tasks invoke `uvx mdformat` directly. `uvx` (from `uv`) runs the tool in an ephemeral virtual environment. This assumes `uv`/`uvx` is available on the system.

**Alternative considered:** `npm:mdformat` -- doesn't exist. Also considered `pipx:mdformat` via mise, but `uvx` is lighter and already available.

### Prefix internal tasks with underscore

The `_require-clean` guard is not meant to be called directly by the user. Prefixing with `_` follows a convention (similar to PaulRBerg's `[private]` annotation in just) to signal it's internal. Mise supports `hide = true` in task metadata for this purpose.

### Install task uses `--global` flag

Skills are installed globally (`--global`) so they appear in the agent home directories (`~/.config/opencode/skills/`, `~/.codex/skills/`). The `skills` CLI creates symlinks from those directories back to the canonical copies in this repo's `skills/` directory.

## Risks / Trade-offs

- **[`uvx` must be available]** The formatting tasks assume `uvx` is installed system-wide. If it's not, `fmt-check` and `fmt-write` will fail. Mitigation: document the dependency in README, or add `uv` to mise tools later.
- **[`skills` CLI symlinking behavior]** We depend on the `skills` CLI's internal behavior of placing canonical copies in `./skills/` and symlinking from agent dirs. If this changes in a future version, the shelf pattern could break. Mitigation: pin the `npm:skills` version in `mise.toml`.
- **[Shelf pattern is manual]** The shelf pattern (activate/deactivate) works by filesystem moves. If a user installs skills without using the mise task (e.g., runs `skills add` directly), shelved skills could reappear. Mitigation: document that skill management should go through mise tasks.
