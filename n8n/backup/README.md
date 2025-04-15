# Workflow Backup Directory

This directory is used by the workflow-importer service to import workflows and credentials into n8n.

## Directory Structure

The workflow-importer service expects the following structure:

```
n8n/backup/
├── workflows.json     # Exported workflows in JSON format (when using --separated flag)
├── credentials/       # Directory containing exported credentials (when using --separated flag)
│   ├── credential1.json
│   ├── credential2.json
│   └── ...
```

## How to Export Workflows and Credentials

To export workflows and credentials from an existing n8n instance:

1. Export workflows:
   ```
   n8n export:workflow --all --separated --output=./n8n/backup/workflows
   ```

2. Export credentials:
   ```
   n8n export:credentials --all --separated --output=./n8n/backup/credentials
   ```

## Importing Process

The workflow-importer service will automatically:

1. Import credentials from the `credentials` directory
2. Import workflows from the `workflows.json` file

This happens when the Docker Compose stack is started.

## Manual Operations

You can also manually import/export workflows and credentials by accessing the n8n container:

```bash
# Access the n8n container
docker exec -it n8n-auto-agent_n8n_1 /bin/sh

# Import workflows
n8n import:workflow --input=/backup/workflows.json

# Import credentials
n8n import:credentials --input=/backup/credentials
```

## Notes

- Make sure to properly encrypt your credentials using the same encryption key as defined in your `.env` file
- The n8n service will start only after the workflow-importer has successfully completed