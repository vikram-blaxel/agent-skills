---
name: blaxel-cli
description: Manage Blaxel resources from the command line using the bl CLI. Deploy agents, sandboxes, jobs, and MCP servers. Also installs the Blaxel CLI if not present.
allowed-tools: Bash(bl:*), Bash(curl:*)
---

# Blaxel CLI

A CLI to manage Blaxel cloud resources from the command line: agents, sandboxes, jobs, MCP servers, drives, and more.

## Prerequisites

The `bl` command must be available on PATH. To check:

```bash
bl version
```

If not installed, install via the official install script:

```bash
curl -fsSL https://raw.githubusercontent.com/blaxel-ai/toolkit/main/install.sh | sh
```

Or via Homebrew:

```bash
brew tap blaxel-ai/blaxel && brew install blaxel
```

After installation, log in to your workspace:

```bash
bl login my-workspace
```

## Global Flags

All commands support these flags:

| Flag | Description |
|------|-------------|
| `-o, --output <format>` | Output format: pretty, yaml, json, table |
| `-w, --workspace <name>` | Override workspace for this command |
| `-v, --verbose` | Enable verbose output |
| `-u, --utc` | Enable UTC timezone |
| `--skip-version-warning` | Skip version warning |

## Non-Interactive Mode

For commands that prompt for input (confirmations, selections), add `-y` or `--yes` to auto-confirm. This is required when running in non-interactive / no-TTY environments (scripts, CI, agents).

## Available Commands

```
bl login              # Authenticate to a workspace
bl logout             # Remove stored credentials
bl workspaces         # List and switch workspaces
bl new                # Create a new resource from template (agent, mcp, sandbox, job, volume-template)
bl serve              # Run resource locally for development
bl deploy             # Build and deploy to Blaxel cloud
bl push               # Build and push image without deploying
bl get                # List or inspect resources
bl delete             # Delete resources
bl apply              # Apply configuration from YAML files
bl run                # Execute a resource with input data
bl chat               # Interactive chat with a deployed agent
bl connect            # Interactive terminal into a sandbox
bl logs               # View resource logs
bl token              # Retrieve authentication token
bl upgrade            # Upgrade CLI to latest version
bl version            # Print version number
bl completion         # Generate shell completion scripts
```

## Reference Documentation

- [login](references/login.md) - Authenticate to a workspace
- [deploy](references/deploy.md) - Build and deploy to Blaxel cloud
- [get](references/get.md) - List or inspect resources
- [new](references/new.md) - Create a new resource from template
- [serve](references/serve.md) - Run resource locally for development
- [run](references/run.md) - Execute a resource with input data
- [chat](references/chat.md) - Interactive chat with a deployed agent
- [logs](references/logs.md) - View resource logs
- [delete](references/delete.md) - Delete resources
- [apply](references/apply.md) - Apply configuration from YAML files
- [push](references/push.md) - Build and push image without deploying
- [connect](references/connect.md) - Interactive terminal into a sandbox
- [workspaces](references/workspaces.md) - List and switch workspaces
- [token](references/token.md) - Retrieve authentication token

## Discovering Options

To see available subcommands and flags, run `--help` on any command:

```bash
bl --help
bl deploy --help
bl get --help
bl get agents --help
```

## Common Workflows

### Deploy an agent

```bash
bl new agent my-agent
cd my-agent
bl serve --hotreload    # Test locally
bl deploy               # Deploy to cloud
bl chat my-agent        # Chat with it
```

### Manage sandboxes

```bash
bl get sandboxes                    # List all
bl get sandbox my-sandbox --watch   # Watch status
bl connect sandbox my-sandbox       # Interactive terminal
bl logs sandbox my-sandbox --follow # Stream logs
bl delete sandbox my-sandbox        # Clean up
```

### Multi-workspace deployment

```bash
bl workspaces dev     # Switch to dev
bl deploy             # Deploy to dev
bl workspaces prod    # Switch to prod
bl deploy             # Deploy to prod
```
