# First Flow - n8n Workflow

## Overview
This is a simple n8n workflow that demonstrates the basic functionality of sending messages to Discord using a webhook. It serves as an introductory example for n8n automation.

## Workflow Structure

### Nodes
1. **Manual Trigger** - Starts the workflow when manually executed
2. **Discord** - Sends a message to Discord using a webhook

### Flow Description
The workflow consists of two connected nodes:
- **When clicking 'Execute workflow'**: A manual trigger that initiates the workflow
- **Discord**: Sends "Hello from n8n" message to a Discord channel via webhook

## Prerequisites

### Discord Setup
1. Create a Discord webhook in your target Discord server:
   - Go to your Discord server settings
   - Navigate to Integrations → Webhooks
   - Create a new webhook and copy the URL

### n8n Credentials
1. Set up Discord Webhook credentials in n8n:
   - Go to n8n credentials page
   - Add new Discord Webhook credential
   - Paste your Discord webhook URL

## Installation

1. Import the workflow file `First_flow__git_.json` into your n8n instance
2. Configure the Discord webhook credentials
3. Test the workflow by clicking "Execute workflow"

## Usage

1. Open the workflow in n8n
2. Click the "Execute workflow" button
3. Check your Discord channel for the "Hello from n8n" message

## Configuration

### Discord Node Parameters
- **Authentication**: Webhook
- **Content**: "Hello from n8n"
- **Webhook ID**: Auto-generated when credentials are set

## Customization

You can modify this workflow by:
- Changing the message content in the Discord node
- Adding additional nodes for more complex automation
- Using different triggers (schedule, webhook, etc.)
- Adding conditional logic or data processing

## File Information
- **Workflow ID**: a6prCRA2TUqAmK3h
- **Version ID**: b43b0d2f-7647-404e-ae60-e6c2c53a16b0
- **Status**: Inactive (needs to be activated for scheduled triggers)

## Next Steps
This basic workflow can be extended to:
- Send notifications based on external events
- Process data before sending to Discord
- Integrate with other services and APIs
- Add error handling and logging