# Getting Started Guide

This guide will help you set up and run the Slack Incident Summarization Bot.

## Prerequisites

- Docker and Docker Compose installed on your system
- AWS account with Bedrock access
- Slack workspace with admin privileges

## Setup Steps

### 1. Environment Configuration

1. Copy the example environment file to create your `.env` file:
   ```bash
   cp .env.example .env
   ```

2. Edit the `.env` file and fill in your actual credentials:
   - Generate an encryption key: `openssl rand -hex 24`
   - Set your PostgreSQL password
   - Add your AWS credentials for Bedrock
   - Add your Slack API credentials
   - Set pgAdmin credentials if needed

### 2. Prepare Workflow Backup (Optional)

If you have existing n8n workflows to import:

1. Export workflows from your existing n8n instance:
   ```bash
   n8n export:workflow --all --separate --output=/backup/workflows
   ```

2. Export credentials from your existing n8n instance:
   ```bash
   n8n export:credentials --all --separate --output=/backup/credentials
   ```

### 3. Start the System

Launch the system using Docker Compose:

```bash
docker-compose up -d
```

This will start all services:
- n8n on http://localhost:5678
- pgAdmin on http://localhost:5050
- Qdrant on http://localhost:6333
- PostgreSQL on port 5432

### 4. Access n8n

1. Open your browser and navigate to http://localhost:5678
2. Log in with the default credentials (if not changed):
   - Email: admin@example.com
   - Password: password

### 5. Configure Slack Integration

1. Create a Slack app in your workspace:
   - Go to https://api.slack.com/apps
   - Click "Create New App" > "From scratch"
   - Name your app and select your workspace

2. Add permissions to your Slack app:
   - Navigate to "OAuth & Permissions"
   - Add the following Bot Token Scopes:
     - `chat:write`
     - `commands`
     - `channels:history`
     - `groups:history`
     - `im:history`
     - `mpim:history`

3. Create a slash command:
   - Navigate to "Slash Commands"
   - Click "Create New Command"
   - Command: `/summarize`
   - Request URL: Your n8n webhook URL (e.g., https://your-domain.com/webhook/slack)
   - Short Description: "Summarize incident thread"
   - Usage Hint: "[optional parameters]"

4. Install the app to your workspace:
   - Navigate to "Install App"
   - Click "Install to Workspace"
   - Authorize the app

5. Copy the Bot User OAuth Token and Signing Secret to your `.env` file:
   - Bot User OAuth Token: `SLACK_OAUTH_TOKEN`
   - Signing Secret: `SLACK_SIGNING_SECRET`

6. Restart the containers to apply the new environment variables:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

### 6. Create the n8n Workflow

1. In n8n, create a new workflow
2. Add a Webhook node as the trigger:
   - Authentication: "Header Auth"
   - Header Name: "x-slack-signature"
   - Header Value: Use an expression to verify Slack signature (see Slack API docs)

3. Add a Function node to parse the Slack request and extract the thread ID

4. Add an HTTP Request node to fetch the thread history from Slack API

5. Add an AWS Bedrock node to analyze the conversation using Claude:
   - Use the prompt structure from ADR-002

6. Add another HTTP Request node to post the summary back to the Slack thread

7. Save and activate the workflow

## Testing the Bot

1. In a Slack channel where the bot is invited, start a conversation thread
2. Add some messages simulating an incident investigation
3. In the thread, type `/summarize`
4. The bot should analyze the conversation and post a summary as a reply

## Troubleshooting

- Check the Docker logs for any errors:
  ```bash
  docker-compose logs n8n
  ```

- Verify your AWS Bedrock and Slack credentials are correct

- Ensure the n8n webhook is accessible from the internet if using with Slack

## Next Steps

- Customize the prompt to better fit your specific incident types
- Add error handling to the n8n workflow
- Set up monitoring for the system
- Implement feedback collection to improve the summaries