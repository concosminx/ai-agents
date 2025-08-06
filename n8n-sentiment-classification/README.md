# Survey Sentiment Classification - n8n Workflow

## Overview

This n8n workflow analyzes survey responses from Google Sheets using OpenAI's GPT model to classify sentiment as Positive, Negative, or Neutral. The workflow processes each response individually and updates the original spreadsheet with the sentiment analysis results.

## Workflow Description

The workflow consists of 10 interconnected nodes:

1. **Manual Trigger** - Starts the workflow when manually executed
2. **Google Sheets (Read)** - Reads survey responses from a Google Sheets document
3. **Split in Batches** - Processes each survey response individually in a loop
4. **Code Node** - Transforms data format for AI processing
5. **Basic LLM Chain** - Sends prompts to the AI model for sentiment analysis
6. **OpenAI Chat Model** - GPT-4.1-mini model for sentiment classification
7. **Structured Output Parser** - Ensures consistent JSON output format
8. **Merge** - Combines original data with AI analysis results
9. **Google Sheets (Update)** - Updates the spreadsheet with sentiment results
10. **Discord** - Sends completion notification (executes once at the end)

## Prerequisites

### Required Credentials

1. **Google Service Account**
   - Google Service Account with Google Sheets API access
   - Read and write permissions for the target spreadsheet
   - Credential name: "Google Service Account account"

2. **OpenAI API Key**
   - Valid OpenAI API account with access to GPT models
   - Credential name: "OpenAi account"

3. **Discord Webhook** (Optional)
   - Discord webhook URL for completion notifications
   - Credential name: "Discord Webhook account"

### Required APIs

- Google Sheets API v4
- OpenAI API (GPT-4.1-mini model)
- Discord Webhook API (optional)

## Configuration

### Google Sheets Configuration

- **Document ID**: `19H5fEzQI13ZF1uumsuL_WmV5Id1830eI0f1I26UElHw`
- **Document Name**: "Survey (Responses)"
- **Sheet Name**: "Form Responses 1" (gid=550948745)
- **Operations**: Read rows → Update rows

### AI Model Configuration

- **Model**: GPT-4.1-mini
- **Prompt**: Analyzes input text and returns sentiment classification
- **Output Format**: Structured JSON with sentiment field

### Expected Data Schema

The Google Sheet should contain these columns:
- `Timestamp` - When the response was submitted
- `response` - The survey response text to analyze
- `Sentiment` - Where AI results will be stored (can be empty initially)
- `row_number` - Unique identifier for each row

## Data Flow

1. **Trigger**: Workflow starts manually
2. **Read**: Fetches all survey responses from Google Sheets
3. **Loop**: Processes each response individually:
   - Transforms data format
   - Sends to AI for sentiment analysis
   - Receives structured sentiment result
   - Merges with original data
   - Updates the spreadsheet row
4. **Complete**: Sends Discord notification when all rows are processed

## AI Prompt

The workflow uses this prompt for sentiment analysis:

```
Please identify the sentiment for the input {{ $json.originalText }} with one value from: Positive, Negative, Neutral.

Return the format as json with the attribute sentiment and the value identified.
```

## Expected Output Format

The AI returns structured JSON:
```json
{
  "sentiment": "Positive" | "Negative" | "Neutral"
}
```

## Usage Instructions

1. **Setup Credentials**:
   - Configure Google Service Account with spreadsheet access
   - Add OpenAI API key
   - Set up Discord webhook (optional)

2. **Prepare Data**:
   - Ensure your Google Sheet has the required columns
   - Verify survey responses are in the "response" column
   - Check that row numbers are properly assigned

3. **Execute Workflow**:
   - Click the manual trigger to start processing
   - Monitor progress through the batch processing
   - Check Discord for completion notification
   - Review updated sentiment results in the spreadsheet

## Error Handling

### Common Issues

- **Authentication Errors**: Verify all API credentials are valid and active
- **Sheet Access**: Ensure service account has edit permissions on the spreadsheet
- **AI Model Limits**: Monitor OpenAI API usage and rate limits
- **Data Format**: Verify response column contains text data for analysis

### Troubleshooting

- **Empty Responses**: The workflow will process empty cells but may return inconsistent results
- **Long Text**: Very long survey responses may exceed AI model context limits
- **Network Issues**: Temporary API failures will cause individual rows to fail but won't stop the entire workflow

## Customization Options

### Modify Sentiment Categories
Update the AI prompt to use different sentiment categories:
```
Please identify the sentiment for the input {{ $json.originalText }} with one value from: Very Positive, Positive, Neutral, Negative, Very Negative.
```

### Add Additional Analysis
Extend the prompt to include:
- Emotion detection
- Topic classification
- Confidence scores
- Key phrase extraction

### Change AI Model
Replace GPT-4.1-mini with other models:
- GPT-4 for higher accuracy
- GPT-3.5-turbo for cost efficiency
- Claude or other providers

### Batch Processing
Modify the batch size in "Split in Batches" node to process multiple items simultaneously (consider API rate limits).

## Performance Considerations

- **Processing Time**: Each survey response requires an individual AI API call
- **API Costs**: OpenAI charges per token - longer responses cost more
- **Rate Limits**: OpenAI has rate limits that may slow processing for large datasets
- **Spreadsheet Updates**: Each row update is a separate Google Sheets API call

## Security Notes

- Keep API keys secure and rotate regularly
- Ensure survey data doesn't contain sensitive personal information
- Consider data privacy regulations when processing survey responses
- Monitor API usage to prevent unexpected charges

## File Structure

- `Survey_sentiments__git_.json` - Main n8n workflow file
- `README.md` - This documentation file

## Version Information

- n8n Workflow Version: v1 execution order
- Google Sheets Node: v4.6
- OpenAI Chat Model: v1.2
- Basic LLM Chain: v1.7
- Split in Batches: v3
- Code Node: v2
- Merge Node: v3.2
- Structured Output Parser: v1.3

## Example Use Cases

- **Customer Feedback Analysis**: Analyze product reviews and feedback
- **Employee Survey Processing**: Process workplace satisfaction surveys
- **Social Media Monitoring**: Classify social media mentions and comments
- **Support Ticket Classification**: Categorize customer support requests by sentiment
- **Market Research**: Analyze open-ended survey responses at scale