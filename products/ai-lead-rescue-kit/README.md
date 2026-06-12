# AI Lead Rescue Kit

Capture, qualify, and follow up with inbound leads using n8n, OpenAI, and Google Sheets.

## What this product does

```text
Website form / webhook lead
-> OpenAI lead qualification
-> Google Sheets CRM row
-> suggested follow-up reply
-> next action and follow-up date
```

## Best for

- solo consultants
- local service businesses
- small agencies
- coaches
- real estate agents
- home service businesses
- freelancers who still track leads in Google Sheets

## Included

```text
workflows/ai-lead-rescue-kit-webhook-to-sheets.n8n.json
google-sheet-template/lead-tracker-schema.csv
google-sheet-template/sample-leads.csv
docs/setup-guide.md
docs/prompt-library.md
docs/troubleshooting.md
docs/handoff-checklist.md
marketing/gumroad-sales-page.md
marketing/fiverr-gig.md
marketing/upwork-pitch.md
marketing/demo-video-script.md
```

## MVP scope

This is a webhook-first workflow. It works with form tools and any app that can send a POST request.

Examples:

- Tally
- Typeform
- Webflow forms via webhook
- Zapier webhook
- Make webhook
- custom website forms

Gmail inbox automation is a recommended upgrade, not part of the base MVP.

## Required accounts

- n8n Cloud or self-hosted n8n
- OpenAI API key
- Google account with Google Sheets access

## Core promise

```text
Stop losing leads in your inbox or messy spreadsheets.
```
