# bl get

Retrieve information about Blaxel resources in your workspace.

## Resource Types

| Type | Aliases | Description |
|------|---------|-------------|
| `agents` | `agent`, `ag` | AI agent applications |
| `functions` | `function`, `fn`, `mcp`, `mcps` | MCP servers (tool providers) |
| `jobs` | `job`, `jb` | Batch processing tasks |
| `sandboxes` | `sandbox`, `sbx` | Isolated execution environments |
| `models` | `model` | AI model configurations |
| `volumes` | `volume` | Persistent storage |
| `drives` | `drive` | Agent Drives (shared filesystem) |
| `policies` | `policy` | Access control policies |
| `integrationconnections` | | External service integrations |
| `image` | | Container image information |
| `volumetemplates` | `volumetemplate` | Volume templates |
| `previews` | `preview` | Sandbox preview URLs |
| `previewtokens` | `previewtoken` | Preview access tokens |

## Usage

```
bl get [command] [flags]
```

## Output Formats

Use `-o` flag: `pretty` (default), `json`, `yaml`, `table`.

## Examples

```bash
# List all agents
bl get agents

# Get specific agent details
bl get agent my-agent

# Get in JSON format (useful for scripting)
bl get agent my-agent -o json

# Watch agent status in real-time
bl get agent my-agent --watch

# Get MCP servers
bl get functions
bl get mcp

# List processes in a sandbox
bl get sandbox my-sandbox process
bl get sbx my-sandbox ps

# List previews for a sandbox
bl get sandbox my-sandbox previews

# List executions for a job
bl get job my-job executions

# Get specific execution
bl get job my-job execution EXECUTION_ID

# Filtering with jq
bl get jobs -o json | jq -r '.[] | select(.status == "DELETING") | .metadata.name'
bl get sandboxes -o json | jq -r '.[] | select(.status == "DEPLOYED") | .metadata.name'
bl get agents -o json | jq -r '.[] | select(.metadata.name | contains("test")) | .metadata.name'
```

## Flags

| Flag | Description |
|------|-------------|
| `--watch` | Continuously monitor a resource for changes |
| `-o, --output <format>` | Output format: pretty, json, yaml, table |
