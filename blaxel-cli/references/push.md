# bl push

Build and push a container image to the Blaxel registry without creating a deployment.

Unlike `bl deploy`, this command does NOT create or update any resource.

## Usage

```
bl push [flags]
```

## Examples

```bash
# Push current directory as an image
bl push

# Push with a custom name
bl push --name my-image

# Push a specific subdirectory
bl push -d ./packages/my-agent

# Push specifying a resource type
bl push --type agent
```

## Flags

| Flag | Description |
|------|-------------|
| `-d, --directory <path>` | Source directory path |
| `--docker-config <path>` | Path to Docker config.json |
| `-n, --name <name>` | Name for the image |
| `-c, --registry-cred <creds>` | Registry credentials |
| `-t, --type <type>` | Resource type: agent, function, sandbox, job |
| `-y, --yes` | Skip interactive mode |
