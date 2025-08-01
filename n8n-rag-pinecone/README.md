# n8n RAG with Pinecone Vector Database

This project contains two n8n workflows that implement a complete RAG (Retrieval-Augmented Generation) system using Pinecone as the vector database. The system automatically ingests documents from Google Drive and provides an intelligent chat interface for querying the stored knowledge.

## Overview

The project includes two complementary workflows:
1. **RAG #1** - Document ingestion pipeline that monitors Google Drive and stores documents in Pinecone
2. **RAG #2** - Chat agent that retrieves information from Pinecone and can create tasks in Airtable

This setup creates a complete knowledge management system where documents are automatically processed and made searchable through a conversational AI interface.

## Files Structure

```
├── RAG__1.json    # Document ingestion workflow (Google Drive → Pinecone)
├── RAG__2.json    # Chat agent workflow (Pinecone retrieval + task creation)
└── README.md      # This file
```

## Workflow Details

### RAG #1: Document Ingestion Pipeline

**Purpose:** Automatically monitors a Google Drive folder and processes new documents into Pinecone vector storage.

**Key Components:**
- **Google Drive Trigger** - Monitors specified folder for new files (runs every 30 minutes)
- **Google Drive Node** - Downloads files from Google Drive
- **Default Data Loader** - Processes binary document data
- **Recursive Character Text Splitter** - Splits documents into manageable chunks
- **OpenAI Embeddings** - Generates vector embeddings for text chunks
- **Pinecone Vector Store** - Stores embeddings in "n8n" index under "Test" namespace

**Supported File Types:** PDF and other document formats supported by the Default Data Loader

### RAG #2: Chat Agent with RAG Capabilities

**Purpose:** Provides a conversational interface that can answer questions using the Pinecone knowledge base and create tasks in Airtable.

**Key Components:**
- **Chat Trigger** - Webhook endpoint for receiving chat messages
- **AI Agent** - Orchestrates the conversation flow with system prompt
- **OpenAI Chat Model (GPT-4o)** - Primary language model for responses
- **Window Buffer Memory** - Maintains conversation context
- **Vector Store Tool** - Retrieves relevant information from Pinecone
- **OpenAI Chat Model (GPT-4o-mini)** - Secondary model for vector store queries
- **Airtable Tool** - Creates tasks when requested
- **Workflow Tool** - Can call other n8n workflows

**Agent Capabilities:**
- Answer questions using the Pinecone knowledge base
- Create tasks in Airtable when requested
- Maintain conversation context across messages
- Professional, customer-service oriented responses

## Setup Instructions

### Prerequisites
- n8n instance running with AI nodes enabled
- Google Drive account with API access
- Pinecone account with vector database
- OpenAI API key
- Airtable account (for task management)

### Required Credentials
You'll need to configure the following credentials in n8n:

1. **Google Drive OAuth2 API** - For accessing Google Drive files
2. **Pinecone API** - For vector database operations
3. **OpenAI API** - For embeddings and chat completions
4. **Airtable Token API** - For creating tasks

### Installation Steps

1. **Import Workflows:**
   - Import `RAG__1.json` into your n8n instance
   - Import `RAG__2.json` into your n8n instance

2. **Configure RAG #1 (Document Ingestion):**
   - Set up Google Drive credentials
   - Configure the Google Drive Trigger to monitor your desired folder
   - Update the Pinecone index name if different from "n8n"
   - Set the Pinecone namespace (currently set to "Test")
   - Configure OpenAI credentials for embeddings

3. **Configure RAG #2 (Chat Agent):**
   - Set up the chat webhook endpoint
   - Configure OpenAI credentials for both chat models
   - Update Pinecone settings to match RAG #1 configuration
   - Set up Airtable credentials and configure the target base/table
   - Customize the system prompt if needed

4. **Test the System:**
   - Activate both workflows
   - Upload a test document to your monitored Google Drive folder
   - Wait for RAG #1 to process the document (up to 30 minutes)
   - Test the chat interface via the webhook endpoint

## Configuration Options

### Document Processing (RAG #1)
- **Trigger Schedule:** Currently set to check every 30 minutes (`*/30 * * * *`)
- **Google Drive Folder:** Update the `folderToWatch` parameter
- **Pinecone Namespace:** Modify in the Pinecone Vector Store node
- **Text Splitting:** Adjust parameters in the Recursive Character Text Splitter

### Chat Agent (RAG #2)
- **System Prompt:** Customize the agent's behavior and personality
- **Models:** Switch between different OpenAI models (GPT-4o, GPT-4o-mini, etc.)
- **Memory:** Adjust the Window Buffer Memory settings for conversation length
- **Tools:** Add or remove tools like Airtable integration

## Usage

### Document Ingestion
1. Upload documents to your configured Google Drive folder
2. RAG #1 will automatically detect and process new files
3. Documents are split, embedded, and stored in Pinecone
4. Monitor the workflow execution for any errors

### Chat Interface
1. Send POST requests to the chat webhook endpoint
2. Include your message in the request body
3. The agent will:
   - Search the Pinecone knowledge base for relevant information
   - Generate contextual responses using the retrieved data
   - Create Airtable tasks if requested
   - Maintain conversation history

### Task Creation
When users request tasks to be created, the agent will:
- Extract task name and notes from the conversation
- Create a new record in Airtable with "Todo" status
- Confirm task creation to the user

## API Integration

The chat agent can be integrated with external applications through the webhook endpoint. Send POST requests with the following structure:

```json
{
  "message": "Your question or request here",
  "sessionId": "unique-session-identifier"
}
```

## Troubleshooting

### Common Issues

1. **Documents Not Processing:**
   - Check Google Drive credentials and permissions
   - Verify the monitored folder ID is correct
   - Ensure the trigger is active and properly scheduled

2. **Chat Responses Missing Context:**
   - Verify Pinecone index and namespace match between workflows
   - Check that documents have been successfully processed by RAG #1
   - Confirm OpenAI embedding models are consistent

3. **Task Creation Failing:**
   - Validate Airtable credentials and permissions
   - Check base and table IDs in the Airtable node
   - Ensure required fields are properly mapped

4. **Performance Issues:**
   - Consider adjusting text chunk sizes in the splitter
   - Monitor OpenAI API usage and rate limits
   - Optimize Pinecone vector search parameters

## Customization

### Adding New Document Sources
Modify RAG #1 to include additional triggers:
- Dropbox, OneDrive, or other cloud storage
- FTP/SFTP servers
- Local file system monitoring
- Webhook-based document uploads

### Extending Agent Capabilities
Add new tools to RAG #2:
- Email notifications
- Calendar integration
- CRM systems
- Additional databases or APIs

### Improving Retrieval Quality
- Experiment with different embedding models
- Adjust text splitting strategies
- Implement metadata filtering in Pinecone
- Add reranking for better search results

## Credits

This project was originally created by [Jono Catliff](https://www.youtube.com/@jonocatliff). Check out his YouTube channel for more AI and automation tutorials.