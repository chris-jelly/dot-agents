## ADDED Requirements

### Requirement: Mise tool configuration
The system SHALL have a `mise.toml` at the repo root that declares `npm:skills` as a managed tool so the `skills` CLI is available without global npm install or `npx`.

#### Scenario: Skills CLI is available via mise
- **WHEN** user runs `mise install` in the repo root
- **THEN** the `skills` command is available in the mise-managed environment

### Requirement: Markdown formatting
The system SHALL provide mise tasks `fmt-check` and `fmt-write` that check and fix markdown formatting using `uvx mdformat`.

A `.mdformat.toml` SHALL exist at the repo root configuring mdformat to exclude `skills/`, `shelf/`, and `node_modules/` directories, and to enable line numbering.

#### Scenario: Check formatting
- **WHEN** user runs `mise run fmt-check`
- **THEN** mdformat checks all markdown files (excluding configured exclusions) and exits non-zero if any are incorrectly formatted

#### Scenario: Fix formatting
- **WHEN** user runs `mise run fmt-write`
- **THEN** mdformat formats all markdown files in-place (excluding configured exclusions)

### Requirement: Directory structure
The repo SHALL contain a `skills/` directory for active skills and a `shelf/` directory for deactivated skills. Both directories SHALL contain a `.gitkeep` file so they are tracked by git even when empty.

#### Scenario: Fresh clone
- **WHEN** user clones the repo
- **THEN** both `skills/` and `shelf/` directories exist and are empty (aside from `.gitkeep`)

### Requirement: Gitignore configuration
The repo SHALL have a `.gitignore` that excludes `node_modules/`, `.DS_Store`, and `.venv/`.

#### Scenario: Node modules ignored
- **WHEN** `npm install` or `npx` creates a `node_modules/` directory
- **THEN** it does not appear in `git status`

### Requirement: README with attribution
The repo SHALL have a `README.md` that briefly explains the purpose (central AI agent skill management), the structure (`skills/`, `shelf/`), basic usage, and credits PaulRBerg/dot-agents as inspiration with a link.

#### Scenario: README exists
- **WHEN** user views the repo root
- **THEN** `README.md` is present and contains a credit/link to `https://github.com/PaulRBerg/dot-agents`
