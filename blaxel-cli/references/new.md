# bl new

Create a new Blaxel resource from templates.

## Resource Types

| Type | Description |
|------|-------------|
| `agent` | AI agent application (chatbots, coding assistants, data analysts) |
| `mcp` | Model Context Protocol server (custom tools, API integrations) |
| `sandbox` | Isolated execution environment |
| `job` | Batch processing task |
| `volumetemplate` | Pre-configured volume template |

## Usage

```
bl new [type] [directory] [flags]
```

## Examples

```bash
# Interactive creation (recommended)
bl new

# Create agent interactively
bl new agent

# Create agent with specific template
bl new agent my-agent -t google-adk-py

# Create MCP server (non-interactive)
bl new mcp my-mcp-server -y -t mcp-py

# Create job with specific template
bl new job my-batch-job -t jobs-py

# Full workflow
bl new agent my-assistant
cd my-assistant
bl serve --hotreload    # Test locally
bl deploy               # Deploy to Blaxel
bl chat my-assistant    # Chat with deployed agent
```

## Flags

| Flag | Description |
|------|-------------|
| `-t, --template <name>` | Template to use (skips interactive prompt) |
| `-y, --yes` | Skip interactive prompts and use defaults |
