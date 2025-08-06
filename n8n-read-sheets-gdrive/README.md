# Read Google Sheets to Discord - n8n Workflow

## Overview

This n8n workflow reads data from a Google Sheets document and sends formatted messages to a Discord channel for each row. It's designed to process transaction data and notify a Discord channel with relevant information.

## Workflow Description

The workflow consists of 4 main nodes:

1. **Manual Trigger** - Starts the workflow when manually executed
2. **Google Sheets** - Reads rows from a specified Google Sheets document
3. **Split in Batches** - Processes each row individually in a loop
4. **Discord** - Sends a formatted message to Discord for each row

## Prerequisites

### Required Credentials

1. **Google Service Account**
   - A Google Service Account with access to Google Sheets API
   - The service account must have read access to the target spreadsheet
   - Credential name: "Google Service Account account"

2. **Discord Webhook**
   - A Discord webhook URL for the target channel
   - Credential name: "Discord Webhook account"

### Required Permissions

- Google Sheets: Read access to the specified spreadsheet
- Discord: Webhook permissions for the target channel

## Configuration

### Google Sheets Node Configuration

- **Authentication**: Service Account
- **Document ID**: `1Fah9DZveBaaJ_25c63ipA7l1KlO_bTS4bYO4l6OgwtE`
- **Document Name**: "Tranzactii" 
- **Sheet Name**: "Sheet1" (gid=0)
- **Operation**: Read rows

### Discord Node Configuration

- **Authentication**: Webhook
- **Message Content**: 
  ```
  My row is {{ $json.row_number }}
  The amount was {{ $json.Suma }}
  ```

## Data Flow

1. The workflow is triggered manually
2. Reads all rows from the specified Google Sheet
3. Each row is processed individually through the batch loop
4. For each row, a Discord message is sent containing:
   - Row number
   - Amount value from the "Suma" column

## Expected Data Format

The Google Sheet should contain at least the following columns:
- `Suma` - The amount/sum value to be reported
- The sheet should have row numbers that can be referenced

## Usage

1. Ensure all required credentials are properly configured in n8n
2. Verify the Google Sheets document is accessible by the service account
3. Set up the Discord webhook in the target channel
4. Execute the workflow manually using the trigger node
5. Monitor the Discord channel for incoming messages

## Troubleshooting

### Common Issues

- **Authentication Error**: Verify Google Service Account credentials and permissions
- **Sheet Not Found**: Check document ID and sheet name configuration
- **Discord Message Failure**: Verify webhook URL and Discord channel permissions
- **Missing Data**: Ensure the "Suma" column exists in the spreadsheet

### Error Handling

The workflow uses basic error handling through the Split in Batches node. If a Discord message fails, the loop will continue processing remaining rows.

## Customization

### Message Format
To customize the Discord message format, modify the content parameter in the Discord node:
```javascript
=My row is {{ $json.row_number }}
The amount was {{ $json.Suma }}
```

### Additional Data Fields
To include more data from the spreadsheet, reference additional column names using `{{ $json.ColumnName }}` syntax.

### Filtering
Add filter nodes between Google Sheets and Split in Batches to process only specific rows based on criteria.

## File Structure

- `Read_sheets_GDrive__git_.json` - The main n8n workflow file
- `README.md` - This documentation file

## Version Information

- n8n Workflow Version: v1 execution order
- Google Sheets Node: v4.6
- Discord Node: v2
- Split in Batches Node: v3
- Manual Trigger Node: v1

## Security Notes

- Keep Google Service Account credentials secure
- Regularly rotate Discord webhook URLs if compromised
- Ensure the Google Sheet doesn't contain sensitive data that shouldn't be posted to Discord
- Consider implementing rate limiting for large datasets to avoid Discord API limits