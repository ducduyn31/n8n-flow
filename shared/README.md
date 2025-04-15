# Shared Data Directory

This directory is mounted to the n8n container at `/data/shared` and can be used for sharing files between the host system and the n8n container.

## Purpose

The shared directory serves several purposes:

1. **Data Exchange**: Easily transfer files between your local system and the n8n container
2. **Persistent Storage**: Store data that should persist across container restarts
3. **External Processing**: Save output files from n8n workflows for further processing by external tools
4. **Input Files**: Provide input files to n8n workflows

## Usage in n8n Workflows

Within your n8n workflows, you can access this directory at `/data/shared`. For example:

- To read a file: `/data/shared/input.json`
- To write a file: `/data/shared/output.json`

## Example Use Cases

1. **Incident Data Storage**: Store incident data extracted from Slack conversations
2. **Report Generation**: Save generated incident reports for external access
3. **Configuration Files**: Store configuration files that can be edited locally but used by n8n
4. **Temporary Data**: Store intermediate processing results

## Best Practices

1. Create subdirectories for different purposes (e.g., `input`, `output`, `config`)
2. Use clear naming conventions for files
3. Consider implementing a cleanup strategy for temporary files
4. Be mindful of permissions when creating files from within the container