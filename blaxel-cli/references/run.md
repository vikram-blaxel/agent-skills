# bl run

Execute a Blaxel resource with custom input data.

## Resource Behavior

| Type | Behavior |
|------|----------|
| `agent` | Send a single request (non-interactive, unlike `bl chat`) |
| `model` | Make an inference request to an AI model |
| `job` | Start a job execution with batch input |
| `function/mcp` | Invoke an MCP server function |

## Usage

```
bl run resource-type resource-name [flags]
```

## Examples

```bash
# Run agent with inline data
bl run agent my-agent --data '{"inputs": "Summarize this text"}'

# Run agent with file input
bl run agent my-agent --file request.json

# Run job with batch file
bl run job my-job --file batches/process-users.json

# Run job locally with concurrent workers
bl run job my-job --local --file batch.json --concurrent 4

# Run model with custom endpoint
bl run model my-model --path /v1/chat/completions --data '{"messages": [...]}'

# Debug mode
bl run agent my-agent --data '{}' --debug
```

## Flags

| Flag | Description |
|------|-------------|
| `-c, --concurrent <n>` | Number of concurrent workers for local job execution (default 1) |
| `-d, --data <json>` | JSON body data |
| `--debug` | Debug mode |
| `--directory <path>` | Directory to run from |
| `-e, --env-file <files>` | Environment file to load |
| `-f, --file <path>` | Input from a file |
| `--header <headers>` | Request headers (`Key: Value` format) |
| `--local` | Run locally (requires `bl serve`) |
| `--method <method>` | HTTP method (default POST) |
| `--params <params>` | Query params |
| `--path <path>` | Path for the request |
| `-p, --port <port>` | Port for local (default 1338) |
| `-s, --secrets <secrets>` | Secrets to pass |
