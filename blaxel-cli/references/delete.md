# bl delete

Delete Blaxel resources from your workspace. Deletion is permanent and cannot be undone.

## Deletable Resource Types

`agent`, `drive`, `function`, `image`, `integrationconnection`, `job`, `model`, `policy`, `preview`, `previewtoken`, `sandbox`, `volume`, `volumetemplate`

## Usage

```
bl delete [command] [flags]
bl delete -f <file> [flags]
```

## Examples

```bash
# Delete by name
bl delete agent my-agent
bl delete sandbox my-sandbox
bl delete job my-job

# Delete multiple resources
bl delete volume vol1 vol2 vol3

# Delete sandbox preview
bl delete sandbox my-sandbox preview my-preview

# Delete from YAML file
bl delete -f my-resource.yaml

# Delete from directory recursively
bl delete -f ./resources/ -R

# Bulk deletion with jq (always preview first!)
bl get jobs -o json | jq -r '.[] | select(.status == "DELETING") | .metadata.name'
bl delete jobs $(bl get jobs -o json | jq -r '.[] | select(.status == "DELETING") | .metadata.name')
```

## Flags

| Flag | Description |
|------|-------------|
| `-f, --filename <path>` | YAML file containing the resource to delete |
| `-R, --recursive` | Process directory recursively |
