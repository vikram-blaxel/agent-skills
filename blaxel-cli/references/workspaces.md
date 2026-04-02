# bl workspaces

List and manage Blaxel workspaces.

A workspace is an isolated environment within Blaxel that contains your resources. The current workspace (marked with `*`) determines where commands operate.

## Usage

```
bl workspaces [workspace] [flags]
```

Aliases: `workspaces`, `ws`, `workspace`

## Examples

```bash
# List all authenticated workspaces
bl workspaces

# Switch to different workspace
bl workspaces production

# Get only the current workspace name
bl workspaces --current

# Use specific workspace for one command
bl get agents --workspace staging

# Multi-workspace workflow
bl workspaces dev        # Switch to dev
bl deploy                # Deploy to dev
bl workspaces prod       # Switch to prod
bl deploy                # Deploy to prod
```

## Flags

| Flag | Description |
|------|-------------|
| `--current` | Display only the current workspace name |
