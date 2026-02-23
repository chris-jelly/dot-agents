## 1. Project scaffolding

- [x] 1.1 Create `skills/` directory with `.gitkeep`
- [x] 1.2 Create `shelf/` directory with `.gitkeep`
- [x] 1.3 Create `.gitignore` (node_modules/, .DS_Store, .venv/)
- [x] 1.4 Create `.mdformat.toml` excluding skills/, shelf/, node_modules/ with line numbering enabled

## 2. Mise configuration

- [x] 2.1 Create `mise.toml` with `[tools]` section declaring `npm:skills`
- [x] 2.2 Create `.mise/tasks/` directory

## 3. Mise task scripts

- [x] 3.1 Create `.mise/tasks/_require-clean` -- git clean guard that aborts on uncommitted changes
- [x] 3.2 Create `.mise/tasks/list` -- display active and shelved skills with color-coded output
- [x] 3.3 Create `.mise/tasks/install` -- install all skills from a repo via `skills` CLI, respecting shelf
- [x] 3.4 Create `.mise/tasks/activate` -- move skills from shelf/ to skills/, depends on _require-clean
- [x] 3.5 Create `.mise/tasks/deactivate` -- move skills from skills/ to shelf/, depends on _require-clean
- [x] 3.6 Create `.mise/tasks/reset` -- purge all skills with confirmation prompt, depends on _require-clean
- [x] 3.7 Create `.mise/tasks/fmt-check` -- check markdown formatting via `uvx mdformat --check`
- [x] 3.8 Create `.mise/tasks/fmt-write` -- format markdown files via `uvx mdformat`

## 4. Documentation

- [x] 4.1 Create `README.md` with purpose, structure overview, basic usage, and credit to PaulRBerg/dot-agents
