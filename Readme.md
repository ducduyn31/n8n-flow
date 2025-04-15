# <placeholder>: On-Call Duty Automation System

## Project Overview

The <placeholder> is a system designed to automate various aspects of on-call duty and incident resolution processes. It aims to help engineers respond more efficiently to incidents, reduce MTTR, and improve communication with stakeholders.

The project follows a phased approach:

1. **Phase 1 (MVP)**: Slack bot that summarizes incident investigation threads
2. **Future Phases**: TBD

## MVP Features

The initial MVP focuses on a Slack bot that:

- Responds to a `/summarize` command in incident investigation threads
- Analyzes the conversation to extract key information:
  - Incident cause
  - Impact
  - Resolution steps
  - Timeline
- Generates a concise summary suitable for management and stakeholders

## Setup Instructions

### Prerequisites

- Docker/Podman and Docker Compose
- AWS account with Bedrock access
- Slack workspace with admin privileges

### Quick Start

1. Clone this repository
2. Configure environment variables
3. Run `docker-compose up`
4. Configure the Slack app and webhook
5. Test the `/summarize` command in a Slack thread

Detailed setup instructions will be provided in the documentation.

## Usage

1. In a Slack incident investigation thread, type `/summarize`
2. The bot will analyze the conversation and generate a summary
3. The summary will be posted as a reply in the thread

## Future Roadmap

After the MVP, the system will be expanded to include:

1. **Automated Diagnostics**
   - Integration with New Relic for metrics and logs
   - Integration with AWS for service status
   - Automated analysis of common incident patterns

2. **Proactive Monitoring**
   - Predictive analysis to identify potential issues
   - Automated remediation for known issues

3. **Knowledge Base Integration**
   - Build a repository of past incidents
   - Suggest solutions based on similar past incidents

## Documentation

- [Architecture Decision Records](./adrs/)