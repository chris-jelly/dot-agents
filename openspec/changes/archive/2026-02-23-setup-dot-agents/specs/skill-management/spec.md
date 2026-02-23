## ADDED Requirements

### Requirement: Install all skills from a repository
The system SHALL provide a mise task `install` that installs all skills from a given GitHub repository using the Vercel `skills` CLI, targeting OpenCode and Codex agents with global scope.

After installation, the task SHALL check the `shelf/` directory and remove any skills that were previously shelved, preserving the user's deactivation decisions across installs.

#### Scenario: Install skills from a new source
- **WHEN** user runs `mise run install owner/repo`
- **THEN** all skills from that repository are installed globally for OpenCode and Codex agents, canonical copies appear in `skills/`, and `.skill-lock.json` is updated

#### Scenario: Install respects shelved skills
- **WHEN** user runs `mise run install owner/repo` and `shelf/` contains a skill named `foo` that is also in the source repo
- **THEN** the `foo` skill is removed after installation, so it does not appear in `skills/`

#### Scenario: No repository argument provided
- **WHEN** user runs `mise run install` without arguments
- **THEN** the task prints a usage message and exits with a non-zero status

### Requirement: List active and shelved skills
The system SHALL provide a mise task `list` that displays all active skills (from `skills/`) and all shelved skills (from `shelf/`) in a readable, color-coded format.

#### Scenario: Skills exist in both directories
- **WHEN** user runs `mise run list` and both `skills/` and `shelf/` contain entries
- **THEN** active skills are listed under a green heading and shelved skills under a yellow heading

#### Scenario: No skills installed
- **WHEN** user runs `mise run list` and both directories are empty
- **THEN** both sections display "(none)"

### Requirement: Activate shelved skills
The system SHALL provide a mise task `activate` that moves one or more skills from `shelf/` to `skills/`.

The task SHALL depend on the `_require-clean` guard.

#### Scenario: Activate a single skill
- **WHEN** user runs `mise run activate foo` and `shelf/foo` exists
- **THEN** `shelf/foo` is moved to `skills/foo` and a confirmation message is printed

#### Scenario: Activate multiple skills
- **WHEN** user runs `mise run activate foo bar`
- **THEN** both `foo` and `bar` are moved from `shelf/` to `skills/`

### Requirement: Deactivate active skills
The system SHALL provide a mise task `deactivate` that moves one or more skills from `skills/` to `shelf/`.

The task SHALL depend on the `_require-clean` guard.

#### Scenario: Deactivate a single skill
- **WHEN** user runs `mise run deactivate foo` and `skills/foo` exists
- **THEN** `skills/foo` is moved to `shelf/foo` and a confirmation message is printed

### Requirement: Reset all skills
The system SHALL provide a mise task `reset` that purges all installed skills, the lock file, and the agent global skill directories.

The task SHALL prompt for confirmation before proceeding. It SHALL depend on the `_require-clean` guard.

#### Scenario: User confirms reset
- **WHEN** user runs `mise run reset` and confirms the prompt
- **THEN** `skills/` contents are removed, `~/.config/opencode/skills/` and `~/.codex/skills/` are cleared, `.skill-lock.json` is deleted, and a message with reinstall instructions is printed

#### Scenario: User declines reset
- **WHEN** user runs `mise run reset` and declines the prompt
- **THEN** no changes are made

### Requirement: Git clean guard
The system SHALL provide a mise task `_require-clean` (prefixed with underscore to indicate internal/private) that aborts with an error if the git working tree has uncommitted staged or unstaged changes.

The `install`, `activate`, `deactivate`, and `reset` tasks SHALL depend on this guard.

#### Scenario: Working tree is clean
- **WHEN** `_require-clean` runs and there are no uncommitted changes
- **THEN** the task exits successfully (zero status)

#### Scenario: Working tree has uncommitted changes
- **WHEN** `_require-clean` runs and there are staged or unstaged changes
- **THEN** the task prints an error message and exits with non-zero status
