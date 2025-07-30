# n8n Streamlit AI Agents

This project contains two Streamlit applications that interface with n8n workflows to create AI-powered chat agents with RAG (Retrieval-Augmented Generation) capabilities.

## Overview

The project includes two different implementations:
1. **Basic Auth Agent** - Simple chat interface with bearer token authentication
2. **Supabase Auth Agent** - Full-featured chat with Supabase user authentication

Both agents connect to n8n workflows that handle LLM interactions and provide intelligent responses based on your data.

## Files Structure

```
├── n8n-streamlit-agent-basic-auth.py     # Simple chat app with bearer token auth
├── n8n-streamlit-agent.py                # Full-featured app with Supabase auth
├── requirements.txt                       # Python dependencies
├── prompts.txt                           # Prompt templates and instructions
├── Supabase_RAG_AI_Agent_Basic_Auth.json # n8n workflow for basic auth
├── Supabase_RAG_AI_Agent_Supabase_Auth.json # n8n workflow for Supabase auth
└── README.md                             # This file
```

## Features

### Basic Auth Agent (`n8n-streamlit-agent-basic-auth.py`)
- Simple chat interface
- Bearer token authentication
- Session management with unique session IDs
- Direct webhook communication with n8n
- Minimal setup requirements

### Supabase Auth Agent (`n8n-streamlit-agent.py`)
- Full user authentication (login/signup) via Supabase
- Persistent user sessions
- Enhanced security
- User management capabilities
- Session-based chat history

## Setup Instructions

### Prerequisites
- Python 3.7+
- n8n instance running
- Supabase project (for the Supabase auth version)
- OpenAI API key (configured in n8n)

### Installation

1. **Clone or download this repository**

2. **Install dependencies:**
   
   See README.md from root

3. **Configure your applications:**

   **For Basic Auth version:**
   - Edit `n8n-streamlit-agent-basic-auth.py`
   - Replace `YOUR_N8N_WEBHOOK_URL_HERE` with your n8n webhook URL
   - Replace `YOUR_BEARER_TOKEN_HERE` with your bearer token

   **For Supabase Auth version:**
   - Edit `n8n-streamlit-agent.py`
   - Replace `YOUR_SUPABASE_PROJECT_URL_HERE` with your Supabase project URL
   - Replace `YOUR_SUPABASE_ANONYMOUS_API_KEY_HERE` with your Supabase anon key
   - Replace `YOUR_N8N_WEBHOOK_URL_HERE` with your n8n webhook URL

4. **Import n8n workflows:**
   - Import the appropriate JSON workflow file into your n8n instance
   - Configure your OpenAI credentials in n8n
   - Set up your data sources for RAG functionality

### Running the Applications

**Basic Auth Version:**
```bash
streamlit run n8n-streamlit-agent-basic-auth.py
```

**Supabase Auth Version:**
```bash
streamlit run n8n-streamlit-agent.py
```

The applications will be available at `http://localhost:8501`

## Usage

### Basic Auth Agent
1. Open the application in your browser
2. Start typing messages in the chat input
3. The agent will respond using the configured LLM and RAG data

### Supabase Auth Agent
1. Open the application in your browser
2. Sign up for a new account or log in with existing credentials
3. Start chatting with the AI agent
4. Your conversation history is maintained per session
5. Use the logout button to end your session

## Configuration

### n8n Workflow Setup
1. Import the appropriate JSON workflow file
2. Configure your OpenAI API credentials
3. Set up your vector database/knowledge base for RAG
4. Deploy the workflow and note the webhook URL

### Environment Variables (Optional)
For better security, consider using environment variables:
- `SUPABASE_URL`
- `SUPABASE_KEY`
- `N8N_WEBHOOK_URL`
- `BEARER_TOKEN`

## Troubleshooting

1. **Connection Issues:**
   - Verify your n8n webhook URL is correct and accessible
   - Check that your n8n workflow is active

2. **Authentication Issues:**
   - Verify Supabase credentials are correct
   - Check bearer token validity

3. **Dependencies:**
   - Ensure all packages from `requirements.txt` are installed
   - Check Python version compatibility

## Credits

This project was originally created by [Cole Medin](https://www.youtube.com/@ColeMedin). Check out his YouTube channel for more AI and automation tutorials.

