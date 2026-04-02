# bl serve

Start a local development server for your Blaxel project.

Runs your agent or MCP server locally for rapid development and testing.

## Supported Languages

- Python (requires `pyproject.toml` or `requirements.txt`)
- TypeScript/JavaScript (requires `package.json`)
- Go (requires `go.mod`)

## Usage

```
bl serve [flags]
```

Aliases: `serve`, `s`, `se`

## Examples

```bash
# Basic serve with hot reload (recommended)
bl serve --hotreload

# Serve on custom port
bl serve --port 8080

# Serve specific subdirectory in monorepo
bl serve -d packages/my-agent

# Serve with environment variables
bl serve -e .env.local

# Serve with secrets
bl serve -s API_KEY=test-key

# Full development workflow
bl serve --hotreload          # Terminal 1: Run server
bl chat my-agent --local      # Terminal 2: Test agent
```

## Flags

| Flag | Description |
|------|-------------|
| `-d, --directory <path>` | Serve from a sub directory |
| `-e, --env-file <files>` | Environment file to load (default `.env`) |
| `-H, --host <host>` | Bind socket to this host (default `0.0.0.0`) |
| `--hotreload` | Watch for changes in the project |
| `-p, --port <port>` | Bind socket to this port (default `1338`) |
| `-r, --recursive` | Serve recursively (default true) |
| `-s, --secrets <secrets>` | Secrets to pass |
