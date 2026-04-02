# bl apply

Apply configuration changes to resources declaratively using YAML files. Similar to `kubectl apply` - creates resources if they don't exist, or updates them if they do.

## Usage

```
bl apply [flags]
```

## Difference from `deploy`

- `apply` manages resource configuration (metadata, settings, specs)
- `deploy` builds and uploads code as container images

## Examples

```bash
# Apply a single resource
bl apply -f agent.yaml

# Apply all resources in directory
bl apply -f ./resources/ -R

# Apply with environment variable substitution
bl apply -f deployment.yaml -e .env.production

# Apply from stdin
cat config.yaml | bl apply -f -

# Apply with secrets
bl apply -f config.yaml -s API_KEY=xxx -s DB_PASSWORD=yyy
```

## YAML Structure

```yaml
apiVersion: blaxel.ai/v1alpha1
kind: Agent
metadata:
  name: my-agent
spec:
  runtime:
    generation: mk3
    image: agent/my-template-agent:latest
    memory: 4096
```

## Flags

| Flag | Description |
|------|-------------|
| `-e, --env-file <files>` | Environment file to load (default `.env`) |
| `-f, --filename <path>` | Path to YAML file to apply |
| `-R, --recursive` | Process directory recursively |
| `-s, --secrets <secrets>` | Secrets to deploy |
