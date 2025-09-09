# AI Calendar Agent - Calendar & Task Management

## Overview
This n8n workflow implements an intelligent AI agent that helps manage calendar events and tasks through natural language conversations. The agent can create, search, and update events in Google Calendar while also managing tasks in Google Sheets, providing a comprehensive productivity assistant.

## Workflow Structure

### Nodes
1. **Chat Trigger** - Receives chat messages and initiates the AI agent
2. **AI Agent** - Central intelligence using OpenAI GPT-4o-mini
3. **OpenAI Chat Model** - Language model for natural language processing
4. **Window Buffer Memory** - Maintains conversation context
5. **Google Calendar (Create)** - Creates new calendar events
6. **Google Calendar (Search)** - Searches for existing events
7. **Google Sheets (Create)** - Adds new tasks to the task management sheet
8. **Google Sheets (Search)** - Searches for existing tasks by due date
9. **Google Sheets (Update)** - Updates existing task information

### Flow Description
The workflow uses a chat interface to interact with users in natural language. The AI agent can:
- **Calendar Management**: Create and search calendar events
- **Task Management**: Create, search, and update tasks in Google Sheets
- **Intelligent Processing**: Understands date formats and task priorities
- **Context Awareness**: Maintains conversation history for better interactions

## Prerequisites

### Google Services Setup
1. **Google Calendar API**:
   - Enable Google Calendar API in Google Cloud Console
   - Create OAuth2 credentials for calendar access
   - Configure calendar permissions

2. **Google Sheets API**:
   - Enable Google Sheets API in Google Cloud Console
   - Create OAuth2 credentials for sheets access
   - Prepare a Google Sheet with columns: Task Name, Description, Due Date, Priority

3. **OpenAI API**:
   - Obtain OpenAI API key
   - Ensure access to GPT-4o-mini model

### n8n Credentials Required
1. **OpenAI API** - For the language model
2. **Google Calendar OAuth2** - For calendar operations
3. **Google Sheets OAuth2** - For task management

## Installation

1. Import the workflow file `AI_Google_Calendar_Agent.json` into your n8n instance
2. Configure all required credentials:
   - OpenAI API credentials
   - Google Calendar OAuth2 credentials
   - Google Sheets OAuth2 credentials
3. Update the Google Calendar email and Google Sheets document ID in the nodes
4. Test the workflow using the chat interface

## Usage

### Calendar Management
- **Create Events**: "Create a meeting with John tomorrow at 2 PM"
- **Search Events**: "What meetings do I have next week?"
- **Event Details**: Events include start time, end time, and summary

### Task Management
- **Create Tasks**: "Add a task to review the quarterly report due next Friday with high priority"
- **Search Tasks**: "Find tasks due on December 15th"
- **Update Tasks**: "Update the project review task priority to medium"

### Supported Date Formats
- All dates must follow MM/DD/YYYY format (e.g., 01/14/2023)
- The agent will parse natural language dates and convert them appropriately

## Configuration

### Agent Instructions
The AI agent is configured with specific instructions:
- Current date awareness for context
- Structured date formatting requirements
- Tool usage guidelines for Google services
- Search-before-update workflow for data integrity

### Task Priority Levels
- **Low**: Non-urgent tasks
- **Medium**: Standard priority tasks  
- **High**: Urgent or important tasks

### Google Sheets Schema
The task management sheet should include these columns:
- **Task Name**: Brief description of the task
- **Description**: Detailed task information
- **Due Date**: Task deadline (MM/DD/YYYY format)
- **Priority**: Low, Medium, or High

## Customization

### Modifying the Agent Behavior
Edit the system message in the AI Agent node to:
- Change the agent's personality or tone
- Add new instructions or capabilities
- Modify date format requirements
- Add new business rules

### Adding New Tools
The workflow can be extended with additional tools:
- Email integration for notifications
- Slack or Discord for team communication
- Additional productivity services

### Calendar Configuration
- Update the calendar email address in both Google Calendar nodes
- Modify event default settings (duration, reminders, etc.)
- Add location or attendee support

### Sheets Configuration
- Change the Google Sheets document ID
- Modify the sheet name and column mappings
- Add new fields for task categorization

## Troubleshooting

### Common Issues
1. **Authentication Errors**: Verify all OAuth2 credentials are properly configured
2. **Date Format Issues**: Ensure dates follow MM/DD/YYYY format
3. **Calendar Access**: Check Google Calendar permissions and sharing settings
4. **Sheet Access**: Verify Google Sheets permissions and document sharing

### Error Messages
- **"Could not find the record"**: Task search failed - check date format and existing data
- **"Calendar access denied"**: Check Google Calendar OAuth2 credentials
- **"Sheet not found"**: Verify Google Sheets document ID and permissions

## Security Considerations

- Use OAuth2 for secure Google services authentication
- Keep OpenAI API keys confidential
- Limit Google Sheets access to necessary users
- Review calendar permissions regularly

## Use Cases

- **Personal Productivity**: Manage personal calendar and task list
- **Team Coordination**: Shared calendar and task management
- **Project Management**: Track project tasks and deadlines
- **Executive Assistant**: AI-powered scheduling and task management

## Limitations

- Requires active internet connection for all API calls
- Limited to text-based interactions through chat interface
- Date parsing limited to MM/DD/YYYY format
- Task updates require existing task search first

## Future Enhancements

- Voice input support
- Multi-calendar management
- Task dependency tracking
- Email notifications and reminders
- Integration with more productivity tools
