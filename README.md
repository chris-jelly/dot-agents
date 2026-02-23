# dot-agents

Central, version-controlled repository for managing AI agent skills across multiple agents (OpenCode, Codex).

## Structure

```
skills/    # Active skills (symlinked to agent directories)
shelf/     # Deactivated skills (preserved but inactive)
.mise/     # Task scripts for skill lifecycle management
```

## Prerequisites

- [mise](https://mise.jdx.dev/) -- task runner and tool manager
- [uv](https://docs.astral.sh/uv/) -- for `uvx mdformat` (markdown formatting)

## Setup

```sh
mise install        # Install tools (skills CLI)
```

## Usage

```sh
mise run install owner/repo   # Install all skills from a GitHub repo
mise run list                 # List active and shelved skills
mise run activate foo         # Move skill from shelf to active
mise run deactivate foo       # Move skill from active to shelf
mise run reset                # Purge all skills (with confirmation)
mise run fmt-check            # Check markdown formatting
mise run fmt-write            # Format markdown files
```

## How It Works

The [Vercel skills CLI](https://github.com/vercel/skills) handles fetching skill repos from GitHub and symlinking them into agent directories (`~/.config/opencode/skills/`, `~/.codex/skills/`). Mise tasks wrap this CLI and add the shelf pattern -- deactivate skills without uninstalling them, and re-activate later.

## Credit

Inspired by [PaulRBerg/dot-agents](https://github.com/PaulRBerg/dot-agents).
