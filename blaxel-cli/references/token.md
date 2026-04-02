# bl token

Retrieve the authentication token for the specified workspace.

The token is automatically managed and refreshed as needed.

## Usage

```
bl token [workspace] [flags]
```

## Examples

```bash
# Get token for current workspace
bl token

# Get token for specific workspace
bl token my-workspace

# Use in scripts
export TOKEN=$(bl token)
```
