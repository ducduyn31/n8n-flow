# Getting Started Guide

This guide will help you set up and run the Slack Incident Summarization Bot.

## Prerequisites

- Docker and Docker Compose installed on your system
- Claude API Key with access to Claude 3.7 Sonnet
- Slack workspace with admin privileges

## Setup Steps

### 1. Environment Configuration

1. Copy the example environment file to create your `.env` file:
   ```bash
   cp .env.example .env
   ```

2. Edit the `.env` file and fill in your actual credentials:
   - Generate an encryption key: `openssl rand -hex 24`
   - Add your Slack API credentials

### 2. Prepare Workflow Backup (Optional)

If you have existing n8n workflows to import:

1. Export workflows from your running n8n container:
   ```bash
   docker-compose exec n8n n8n export:workflow --all --separate --output=/backup/workflows
   ```

2. Export credentials from your running n8n container:
   ```bash
   docker-compose exec n8n n8n export:credentials --all --separate --output=/backup/credentials
   ```

3. The exported files will be available in the `./n8n/backup` directory on your host machine due to the volume mapping in docker-compose.yml.

### 3. Start the System

Launch the system using Docker Compose:

```bash
docker-compose up -d
```

This will start all services:
- n8n on http://localhost:5678

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
     - `app_mentions:read`
     - `channels:history`
     - `channels:read`
     - `chat:write`
     - `commands`
     - `files:read`
     - `groups:history`
     - `groups:read`
     - `im:history`
     - `im:read`
     - `mpim:history`
     - `mpim:read`
     - `users.profile:read`
     - `users:read`

3. Create a interactivity behavior:
   - Navigate to "Interactivity & Shortcuts"
   - Request URL: Your n8n webhook URL (e.g., https://your-domain.com/webhook/slack)
   - Click "Create New Shortcut"
   - Select "On messages" 
   - Name: "Explain Incident"
   - Short Description: "Quickly summarizes an incident by highlighting what happened, where, who was involved, and the outcome."
   - Callback ID: "summarize_incident"
   - Click "Create"

   *Note:* if you are looking for a local tunneling tool for the webhook, [localtunnel](https://theboroer.github.io/localtunnel-www/) is a great choice.

4. Install the app to your workspace:
   - Navigate to "Install App"
   - Click "Install to Workspace"
   - Authorize the app

5. Copy the Bot User OAuth Token and Signing Secret to your `.env` file:
   - Bot User OAuth Token: `SLACK_OAUTH_TOKEN`

6. Restart the containers to apply the new environment variables:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

## Testing the Bot

1. In a Slack channel where the bot is invited, start a conversation thread
2. Add some messages simulating an incident investigation
3. In the thread, click on the triple dots and select `Explain Incident`. If you cannot find it, click on more shortcuts to list all the available options and find `Explain Incident`.
4. The bot should analyze the conversation and send you the summary.

## Troubleshooting

- Check the Docker logs for any errors:
  ```bash
  docker-compose logs n8n
  ```

- Ensure the n8n webhook is accessible from the internet if using with Slack

## Next Steps

- Customize the prompt to better fit your specific incident types
- Add error handling to the n8n workflow
- Set up monitoring for the system