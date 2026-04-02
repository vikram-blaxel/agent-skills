# bl login

Authenticate with Blaxel to access your workspace.

A workspace is your organization's isolated environment in Blaxel that contains all your resources (agents, jobs, sandboxes, models, etc.). You must login before using most Blaxel CLI commands.

## Authentication Methods

1. **Browser OAuth** (default) - Interactive login via web browser
2. **API Key** - For automation and scripts (set `BL_API_KEY` environment variable)
3. **Client Credentials** - For CI/CD pipelines (set `BL_CLIENT_CREDENTIALS`)

The CLI automatically detects which authentication method to use:
- If `BL_CLIENT_CREDENTIALS` is set, uses client credentials
- If `BL_API_KEY` is set, uses API key authentication
- Otherwise, shows interactive menu to choose browser or API key login

Credentials are stored securely in your system's credential store and persist across sessions.

## Usage

```
bl login [workspace] [flags]
```

## Examples

```bash
# Interactive login (shows menu to choose method)
bl login my-workspace

# Login without workspace (will prompt for workspace)
bl login

# API key authentication (non-interactive)
export BL_API_KEY=your-api-key
bl login my-workspace

# Client credentials for CI/CD
export BL_CLIENT_CREDENTIALS=your-credentials
bl login my-workspace
```

## Flags

| Flag | Description |
|------|-------------|
| `-h, --help` | Help for login |
