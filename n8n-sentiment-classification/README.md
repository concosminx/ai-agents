# Survey Sentiment Classification - n8n Workflow

## Overview
This n8n workflow classifies survey responses from Google Sheets as Positive, Negative, or Neutral using OpenAI, and updates the sheet with results. It can also send notifications to Discord.

## Setup instructions
1. Import `Survey_sentiments__git_.json` into n8n.
2. Set up Google Service Account, OpenAI API, and Discord webhook credentials.
3. Run the workflow to process survey responses and update sentiment results.