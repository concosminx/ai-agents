# n8n AI Leads Form with Intelligent Scoring

This project contains an n8n workflow that implements an intelligent lead qualification system. The workflow automatically collects leads through a web form, scores them using AI, stores qualified leads in Google Sheets, and sends personalized follow-up emails.

## Overview

The AI Leads Form workflow creates a complete lead management pipeline that:
1. **Collects lead information** through a customizable web form
2. **Scores leads intelligently** using OpenAI's GPT model based on budget, email domain, and likelihood to hire
3. **Filters high-quality leads** (score ≥ 3) for immediate follow-up
4. **Stores all leads** in Google Sheets for tracking and analysis
5. **Sends automated emails** to both qualified leads and internal notifications

This system helps businesses automatically identify and prioritize the most promising leads while maintaining organized records and timely communication.

## Files Structure

```
├── AI_Leads_from_Form__git_.json    # Complete n8n workflow
└── README.md                        # This file
```

## Workflow Details

### Form Collection & Data Storage

**Form Trigger Node:**
- Creates a web form titled "Quote" with the following fields:
  - First Name (text input)
  - Last Name (text input)
  - Email (text input)
  - Budget (text input)
  - How likely are you to hire (dropdown: "Not likely" / "Very likely")

**Google Sheets Integration:**
- Automatically stores all form submissions in a Google Sheets document
- Updates existing records based on email address to prevent duplicates
- Maintains complete lead database for analysis and follow-up

### AI-Powered Lead Scoring

**OpenAI Scoring System:**
The workflow uses GPT-4.1-mini to intelligently score leads on a 1-5 scale based on:

1. **Budget Analysis (3 points):** Awards 3 points if budget exceeds $1,000
2. **Email Domain Validation (1 point):** Awards 1 point for business email domains (excludes Gmail, Yahoo, etc.)
3. **Purchase Intent (1 point):** Awards 1 point if lead indicates "Very likely" to hire

**Scoring Prompt:**
```
You're an intelligent bot capable of scoring new leads. Please assign points on a 1-5 rating based on these categories:
1. If the budget is over 1000, please award 3 points
2. If the email of the client is a business email, please award 1 point. Do not award points if its a gmail or yahoo email
3. How likely they are to hire - award 1 point if they're very likely to hire

Please only generate as an output 1 key for "score"
```

### Automated Follow-up System

**Lead Qualification Filter:**
- Only leads scoring 3 or higher proceed to automated follow-up
- Ensures focused attention on most promising prospects

**Personalized Email Responses:**
- Sends thankyou email to qualified leads using their first name
- Includes calendar booking link for immediate engagement
- Professional, branded communication

**Internal Notifications:**
- Sends detailed lead information to internal team
- Includes all form data and scoring results
- Enables quick internal follow-up and coordination

## Setup Instructions

### Prerequisites
- n8n instance running with form trigger capabilities
- Google Sheets API access
- OpenAI API key
- Gmail account for email automation

### Required Credentials
Configure the following credentials in n8n:

1. **OpenAI API** - For intelligent lead scoring
2. **Google Sheets API** - For lead data storage and management
3. **Gmail OAuth2** - For automated email communications

### Installation Steps

1. **Import Workflow:**
   - Import `AI_Leads_from_Form__git_.json` into your n8n instance

2. **Configure Form Settings:**
   - Customize the form title and fields as needed
   - Note the webhook URL for form deployment
   - Test form submission to ensure data collection works

3. **Set up Google Sheets Integration:**
   - Create a new Google Sheets document for lead storage
   - Update the document ID in the "Append or update row in sheet" node
   - Configure column mapping to match your form fields
   - Set email as the matching column to prevent duplicates

4. **Configure AI Scoring:**
   - Add your OpenAI API credentials
   - Customize scoring criteria in the system prompt if needed
   - Adjust the filter threshold (currently set to ≥ 3) based on your needs

5. **Set up Email Automation:**
   - Configure Gmail credentials for sending emails
   - Customize email templates in both "Send a message" nodes
   - Update recipient addresses for internal notifications
   - Add your calendar booking link to customer emails

6. **Test the Complete Flow:**
   - Submit test data through the form
   - Verify data appears in Google Sheets
   - Check that emails are sent for qualified leads
   - Confirm internal notifications are received

## Configuration Options

### Form Customization
- **Form Fields:** Add, remove, or modify form fields in the Form Trigger node
- **Form Title:** Update the form title for branding
- **Field Types:** Use text inputs, dropdowns, checkboxes, etc.
- **Validation:** Add required field validation as needed

### Scoring Algorithm
- **Budget Threshold:** Modify the $1,000 budget threshold for your market
- **Email Domain Rules:** Customize business email detection logic
- **Scoring Weights:** Adjust point values for different criteria
- **Filter Threshold:** Change the minimum score for follow-up (currently 3)

### Email Templates
- **Customer Email:** Personalize the thank you message and branding
- **Internal Notification:** Customize the lead summary format
- **Subject Lines:** Update email subjects for better engagement
- **Calendar Integration:** Add your specific booking platform link

## Usage

### Lead Collection
1. Share the form webhook URL on your website or marketing materials
2. Leads fill out the form with their contact information and requirements
3. All submissions are automatically processed through the workflow

### Lead Management
1. Monitor your Google Sheets document for new leads
2. Review AI scores to prioritize follow-up activities
3. Track email responses and engagement
4. Use the data for sales pipeline analysis

### Performance Monitoring
1. Check workflow execution logs for any errors
2. Monitor form submission rates and scoring distribution
3. Track email delivery and response rates
4. Analyze lead quality trends over time

## Workflow Logic

The complete flow follows this sequence:

1. **Form Submission** → Lead data collected via web form
2. **Data Storage** → Information saved to Google Sheets
3. **AI Scoring** → Lead quality assessed using OpenAI
4. **Quality Filter** → Only high-scoring leads (≥3) proceed
5. **Customer Email** → Personalized thank you message sent
6. **Internal Alert** → Team notification with lead details

## API Integration

The form can be embedded in any website or integrated with other systems using the webhook URL provided by the Form Trigger node.

## Troubleshooting

### Common Issues

1. **Form Not Submitting:**
   - Check webhook URL is correctly configured
   - Verify form trigger is active
   - Test form submission manually

2. **Google Sheets Not Updating:**
   - Confirm Google Sheets credentials are valid
   - Check document ID and sheet name
   - Verify column mapping matches form fields

3. **AI Scoring Errors:**
   - Validate OpenAI API credentials and quota
   - Check prompt format and scoring logic
   - Monitor API response format

4. **Email Delivery Issues:**
   - Verify Gmail credentials and permissions
   - Check email addresses are valid
   - Monitor for spam folder delivery

5. **Filter Not Working:**
   - Confirm filter condition matches AI output format
   - Check threshold value (currently set to 3)
   - Verify JSON path for score extraction

## Customization Ideas

### Enhanced Scoring
- Add industry-specific scoring criteria
- Implement company size evaluation
- Include geographic scoring factors
- Add urgency/timeline assessment

### Advanced Automation
- Integrate with CRM systems
- Add SMS notifications for high-value leads
- Implement lead routing based on criteria
- Add calendar auto-booking for top prospects

### Analytics & Reporting
- Create lead source tracking
- Implement conversion rate monitoring
- Add automated performance reports
- Include A/B testing for form variations

## Security Considerations

- Keep API credentials secure and regularly rotated
- Use HTTPS for form submissions
- Implement rate limiting to prevent spam
- Consider adding CAPTCHA for public forms
- Regularly backup Google Sheets data