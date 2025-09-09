# Contextual Retrieval RAG Agent

## Overview
This n8n workflow implements an advanced RAG (Retrieval-Augmented Generation) system with contextual retrieval capabilities. The system automatically monitors Google Drive for document changes, processes them with enhanced contextual embeddings, and provides an intelligent chat interface for querying the stored knowledge. This approach significantly improves search retrieval accuracy by providing context for each document chunk.

## Workflow Structure

### Main Components

#### Document Processing Pipeline
1. **File Created Trigger** - Monitors Google Drive folder for new files (every minute)
2. **File Updated Trigger** - Monitors Google Drive folder for file updates (every minute)
3. **Loop Over Items** - Processes multiple files in batches
4. **Set File ID** - Extracts file metadata (ID, type, title, URL)
5. **Delete Old Doc Records** - Removes existing document records from database
6. **Download File** - Downloads files from Google Drive with text conversion
7. **Extract Document Text** - Extracts text content from documents
8. **Create Chunks From Doc** - Custom JavaScript chunking (400 chars per chunk)
9. **Chunks To List** - Splits chunks into individual items
10. **Generate Contextual Text** - Creates contextual descriptions for each chunk
11. **Get Values** - Combines original chunk with contextual text
12. **Default Data Loader** - Prepares documents for vector storage
13. **Postgres PGVector Store** - Stores enhanced embeddings in PostgreSQL

#### Chat Interface
1. **Chat Trigger** - Public webhook endpoint for chat messages
2. **RAG AI Agent** - Central AI agent with specialized RAG instructions
3. **OpenAI Chat Model** - Primary language model (GPT-4.1-mini)
4. **Postgres Chat Memory** - Session-based conversation memory
5. **Document RAG Tool** - Vector search tool for document retrieval
6. **Embeddings OpenAI** - Text embedding generation for queries

### Key Innovation: Contextual Retrieval

The workflow implements **contextual retrieval** by:
1. **Chunking documents** into 400-character segments
2. **Generating contextual descriptions** for each chunk using GPT-4.1-nano
3. **Combining context with original content** before embedding
4. **Storing enhanced embeddings** that include both content and situational context

This approach dramatically improves retrieval accuracy by providing semantic context for each document fragment.

## Prerequisites

### Google Services Setup
1. **Google Drive API**:
   - Enable Google Drive API in Google Cloud Console
   - Create OAuth2 credentials for drive access
   - Configure folder permissions for monitoring

### Database Setup
1. **PostgreSQL with pgvector**:
   - Set up PostgreSQL database with pgvector extension
   - Create `documents_pg` table for vector storage
   - Configure connection credentials

### OpenAI API Setup
1. **OpenAI API Access**:
   - Obtain OpenAI API key
   - Ensure access to GPT-4.1-mini and GPT-4.1-nano models
   - Configure text-embedding-3-small model access

### n8n Credentials Required
1. **OpenAI API** - For language models and embeddings
2. **Google Drive OAuth2** - For file monitoring and downloading
3. **PostgreSQL** - For vector storage and chat memory

## Installation

1. Import the workflow file `Contextual_Retrieval_RAG_Agent.json` into your n8n instance
2. Configure all required credentials:
   - OpenAI API credentials
   - Google Drive OAuth2 credentials  
   - PostgreSQL credentials (two instances: one for vector store, one for memory)
3. Update the Google Drive folder ID in the trigger nodes
4. Configure the PostgreSQL table name if different from `documents_pg`
5. Test the document processing by adding a file to the monitored folder
6. Test the chat interface using the webhook endpoint

## Usage

### Document Processing
The system automatically:
- **Monitors** the specified Google Drive folder every minute
- **Processes** new and updated documents immediately
- **Chunks** documents into optimal sizes for retrieval
- **Generates** contextual descriptions for better search
- **Stores** enhanced embeddings in PostgreSQL
- **Maintains** document metadata (file ID, title, URL)

### Chat Interface
Users can:
- **Ask questions** about uploaded documents
- **Get accurate answers** with improved retrieval
- **Maintain conversation** context across sessions
- **Reference specific** documents through metadata

### Supported File Types
- Google Docs (converted to plain text)
- PDF files
- Text files
- Other formats supported by n8n's Extract Document Text node

## Configuration

### Google Drive Monitoring
```json
{
  "folderToWatch": "1iLcA4aE4rOe87BNXhiArRaHkG9dfzJqw",
  "pollTimes": "everyMinute",
  "events": ["fileCreated", "fileUpdated"]
}
```

### Document Chunking Parameters
```javascript
{
  "chunkSize": 400,
  "chunkOverlap": 0,
  "method": "custom_javascript"
}
```

### Contextual Prompt Template
```
<document> 
{{ document_content }} 
</document>
Here is the chunk we want to situate within the whole document 
<chunk> 
{{ chunk_content }}
</chunk> 
Please give a short succinct context to situate this chunk within the overall document for the purposes of improving search retrieval of the chunk. Answer only with the succinct context and nothing else.
```

### AI Agent System Message
The RAG agent is configured with specialized instructions for:
- Providing accurate, document-based responses
- Maintaining neutral, factual language
- Handling cases where information isn't found
- Combining comprehensiveness with conciseness

## Database Schema

### documents_pg Table
- **content** - Combined contextual text and original chunk
- **metadata** - File information (file_id, file_title, file_url)
- **embedding** - Vector representation of the content

### Chat Memory
- Session-based storage in PostgreSQL
- Maintains conversation history per session ID
- Enables context-aware responses

## Advanced Features

### Contextual Enhancement
- Each document chunk gets contextual description
- Improves semantic search accuracy
- Reduces irrelevant retrieval results
- Maintains document structure awareness

### Automatic Cleanup
- Removes old document records when files are updated
- Prevents duplicate embeddings
- Maintains data consistency

### Session Management
- Unique session IDs for each conversation
- Persistent memory across chat sessions
- Context-aware follow-up questions

### Error Handling
- Graceful handling of unsupported file types
- Proper error messages when information isn't found
- Fallback responses for failed retrievals

## Customization

### Modifying Chunk Size
Edit the JavaScript code in "Create Chunks From Doc":
```javascript
const chunkSize = 400; // Adjust as needed
const chunkOverlap = 0; // Add overlap if desired
```

### Changing Contextual Prompt
Modify the prompt in "Generate Contextual Text" node to adjust how context is generated.

### Adjusting Retrieval Parameters
Configure the Document RAG Tool:
- Search results count
- Similarity threshold
- Metadata filtering

### Database Configuration
- Change table names in PGVector nodes
- Modify vector dimensions if using different embedding models
- Adjust PostgreSQL connection parameters

## Troubleshooting

### Common Issues
1. **File Processing Errors**: Check Google Drive permissions and file formats
2. **Database Connection**: Verify PostgreSQL credentials and pgvector extension
3. **Embedding Failures**: Confirm OpenAI API key and model access
4. **Memory Issues**: Check PostgreSQL chat memory table setup

### Performance Optimization
- Monitor document processing time for large files
- Adjust polling frequency based on usage patterns
- Consider chunk size optimization for your use case
- Implement cleanup routines for old embeddings

## Security Considerations

- Use OAuth2 for secure Google Drive access
- Keep OpenAI API keys confidential
- Secure PostgreSQL database connections
- Limit Google Drive folder access to necessary users
- Implement rate limiting for chat endpoints

## Use Cases

- **Knowledge Base**: Company documentation and policies
- **Research Assistant**: Academic papers and research materials
- **Support System**: Product manuals and troubleshooting guides
- **Content Discovery**: Large document collections
- **Legal Research**: Contracts and legal documents

## Limitations

- Requires internet connection for all API calls
- Processing time depends on document size and complexity
- Limited to text-based document content
- OpenAI API rate limits may affect processing speed
- PostgreSQL storage requirements grow with document volume

## Future Enhancements

- Multi-language document support
- Advanced metadata extraction
- Document versioning and change tracking
- Real-time collaboration features
- Integration with more document sources
- Custom embedding model support
- Advanced analytics and usage metrics
