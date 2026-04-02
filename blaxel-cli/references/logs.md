# bl logs

View logs for Blaxel resources.

## Resource Types (with aliases)

| Type | Aliases |
|------|---------|
| `sandboxes` | `sandbox`, `sbx` |
| `jobs` | `job`, `j`, `jb` |
| `agents` | `agent`, `ag` |
| `functions` | `function`, `fn`, `mcp`, `mcps` |

## Usage

```
bl logs RESOURCE_TYPE RESOURCE_NAME [NESTED_ARGS...] [flags]
```

## Examples

```bash
# View logs for a sandbox (last 1 hour default)
bl logs sandbox my-sandbox

# View logs for a specific process in a sandbox
bl logs sandbox my-sandbox my-process

# Stream logs in real-time
bl logs sandbox my-sandbox --follow

# View logs for a job execution
bl logs job my-job exec-abc123

# View logs for a specific task
bl logs job my-job exec-abc123 task-456

# View logs from last 3 days
bl logs job my-job --period 3d

# View logs for a time range
bl logs agent my-agent --start 2024-01-01T00:00:00Z --end 2024-01-01T23:59:59Z

# Filter by severity
bl logs agent my-agent --severity ERROR,FATAL

# Search for specific text
bl logs agent my-agent --search "error"

# Hide timestamps
bl logs agent my-agent --no-timestamps
```

## Flags

| Flag | Description |
|------|-------------|
| `-f, --follow` | Follow log output (like `tail -f`) |
| `-p, --period <duration>` | Time period (e.g., `3d`, `1h`, `10m`) |
| `--start <time>` | Start time (RFC3339 or YYYY-MM-DD) |
| `--end <time>` | End time (RFC3339 or YYYY-MM-DD) |
| `--severity <levels>` | Filter: FATAL,ERROR,WARNING,INFO,DEBUG,TRACE,UNKNOWN |
| `--search <text>` | Filter logs by text content |
| `--no-timestamps` | Hide timestamps |
| `--utc` | Display timestamps in UTC |
