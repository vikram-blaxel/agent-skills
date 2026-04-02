# bl deploy

Deploy your Blaxel project to the cloud.

This command packages your code, builds a container image, and deploys it to your workspace.

## Deployment Process

1. Reading configuration from `blaxel.toml`
2. Packaging source code (respects `.blaxelignore`)
3. Building container image with your runtime and dependencies
4. Uploading to Blaxel's container registry
5. Creating or updating the resource in your workspace
6. Streaming build and deployment logs (interactive mode)

## Usage

```
bl deploy [flags]
```

Aliases: `deploy`, `d`, `dp`

## Examples

```bash
# Basic deployment (interactive mode with live logs)
bl deploy

# Non-interactive deployment (for CI/CD)
bl deploy --yes

# Deploy with environment variables
bl deploy -e .env.production

# Deploy with command-line secrets
bl deploy -s API_KEY=xxx -s DB_PASSWORD=yyy

# Deploy without rebuilding (reuse existing image)
bl deploy --skip-build

# Dry run to validate configuration
bl deploy --dryrun

# Deploy specific subdirectory in monorepo
bl deploy -d ./packages/my-agent

# Deploy specifying a resource type
bl deploy --type sandbox

# Recursively deploy all projects in monorepo
bl deploy -R
```

## Flags

| Flag | Description |
|------|-------------|
| `-d, --directory <path>` | Deployment app path, can be a sub directory |
| `--docker-config <path>` | Path to a Docker config.json file with registry credentials |
| `--dryrun` | Dry run the deployment |
| `-e, --env-file <files>` | Environment file to load (default `.env`) |
| `--experimental` | Enable experimental features |
| `-n, --name <name>` | Optional name for the deployment |
| `-r, --recursive` | Deploy recursively (default true) |
| `-c, --registry-cred <creds>` | Registry credentials (format: registry=username:password) |
| `-s, --secrets <secrets>` | Secrets to deploy |
| `--skip-build` | Skip the build step |
| `-t, --type <type>` | Resource type: sandbox, agent, function, job |
| `-y, --yes` | Skip interactive mode |
