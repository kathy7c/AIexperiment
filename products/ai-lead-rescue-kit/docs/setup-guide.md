# AI Lead Rescue Kit Setup Guide

## Outcome

After setup, every new lead sent to the webhook will be:

1. normalized into a consistent lead record
2. qualified by OpenAI
3. assigned a lead type: Hot / Warm / Cold / Spam / Not a Lead
4. given a suggested reply
5. logged into Google Sheets

## Step 1: Create the Google Sheet

Create a Google Sheet named:

```text
AI Lead Rescue Kit - Lead Tracker
```

Create a tab named:

```text
Leads
```

Paste this header row into row 1:

```text
Date
Source
Lead Name
Email
Phone
Company
Original Message
Need Summary
Lead Type
Urgency
Budget Signal
Problem Category
AI Qualification
Suggested Reply
Next Action
Follow-up Date
Status
Confidence Score
Notes
```

You can also use:

```text
google-sheet-template/lead-tracker-schema.csv
```

## Step 2: Import the n8n workflow

Import:

```text
workflows/ai-lead-rescue-kit-webhook-to-sheets.n8n.json
```

## Step 3: Add your OpenAI API key

Open this node:

```text
Analyze Lead With OpenAI
```

Replace:

```text
Bearer REPLACE_WITH_OPENAI_API_KEY
```

with:

```text
Bearer YOUR_OPENAI_API_KEY
```

Keep the `Bearer ` prefix.

Default model:

```text
gpt-4o-mini
```

## Step 4: Connect Google Sheets

Open this node:

```text
Append Lead To Google Sheet
```

Replace:

```text
REPLACE_WITH_GOOGLE_SHEET_ID
```

with your Sheet ID.

Example:

```text
https://docs.google.com/spreadsheets/d/SHEET_ID_HERE/edit
```

Connect your Google Sheets credential in n8n.

## Step 5: Test with sample data

Run:

```text
Manual Trigger
```

Expected result:

```text
One new row appears in the Leads tab.
```

## Step 6: Test the webhook

Open the `Lead Webhook` node and copy the Test URL.

Send a POST request with JSON:

```json
{
  "source": "Website Form",
  "name": "Sarah Miller",
  "email": "sarah@example.com",
  "phone": "+1 555 0134",
  "company": "Miller Home Services",
  "message": "Hi, I need help setting up weekly cleaning for a 4-bedroom house starting next week. Can you send pricing and availability?"
}
```

Any form tool that can send a webhook can use the same field names.

## Recommended form fields

```text
name
email
phone
company
message
source
```

Only `message` is required. The workflow will still run if other fields are missing.

## Step 7: Activate

After the test row looks correct:

1. activate the workflow
2. copy the Production URL from `Lead Webhook`
3. paste that URL into your form tool

## Human review rule

This workflow creates a suggested reply. It does not send emails automatically.

Review replies before sending.
