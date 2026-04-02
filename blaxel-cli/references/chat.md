# bl chat

Start an interactive chat session with a deployed agent.

The agent must be deployed and in DEPLOYED status. Use `bl get agent NAME` to check status.

## Usage

```
bl chat [agent-name] [flags]
```

## Examples

```bash
# Chat with deployed agent
bl chat my-agent

# Chat with local development agent (requires 'bl serve')
bl chat my-agent --local

# Debug mode (shows API calls and responses)
bl chat my-agent --debug

# Add custom headers
bl chat my-agent --header "X-User-ID: 123"

# Development workflow
bl serve --hotreload         # Terminal 1
bl chat my-agent --local     # Terminal 2
```

## Flags

| Flag | Description |
|------|-------------|
| `--debug` | Debug mode |
| `--header <headers>` | Request headers (`Key: Value` format) |
| `--local` | Run locally |
| `-p, --port <port>` | Port for local (default 1338) |
