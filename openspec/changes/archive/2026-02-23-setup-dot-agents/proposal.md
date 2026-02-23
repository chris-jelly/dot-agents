## Why

This repo needs to become a functional dot-agents project -- a central, version-controlled repository for managing AI agent skills across multiple agents (OpenCode, Codex). Currently it's an empty git repo with only local OpenSpec/OpenCode configuration. There's no skill management, no task runner, no formatting, and no structure for the skills + shelf pattern.

## What Changes

- Add `mise.toml` with tool definitions (`npm:skills` for the Vercel skills CLI, `npm:mdformat` or system `uvx mdformat` for markdown formatting) and task configuration
- Add file-based mise tasks in `.mise/tasks/` for skill lifecycle management: `install`, `list`, `activate`, `deactivate`, `reset`, `fmt-check`, `fmt-write`, plus a `_require-clean` guard
- Create the `skills/` and `shelf/` directory structure for the active/inactive skill pattern
- Add `.skill-lock.json` tracking (starts empty, populated when skills are installed)
- Add `.mdformat.toml` for markdown formatting configuration
- Add `.gitignore` for node_modules, .DS_Store, etc.
- Add a minimal `README.md` with credit to PaulRBerg/dot-agents as inspiration

## Capabilities

### New Capabilities
- `skill-management`: Mise tasks wrapping the Vercel `skills` CLI for installing, listing, activating, deactivating, and resetting skills across agents (OpenCode, Codex)
- `project-setup`: Core repo structure including mise.toml, .gitignore, .mdformat.toml, README, and directory scaffolding (skills/, shelf/)

### Modified Capabilities

## Impact

- New files at repo root: `mise.toml`, `.mdformat.toml`, `.gitignore`, `README.md`
- New directory: `.mise/tasks/` with task scripts
- New directories: `skills/`, `shelf/` (both start empty, populated by `mise run install`)
- Dependencies: `npm:skills` (Vercel skills CLI), managed by mise
