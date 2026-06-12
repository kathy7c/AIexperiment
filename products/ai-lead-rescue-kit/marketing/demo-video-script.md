# Demo Video Script

Target length:

```text
2-3 minutes
```

## Opening

```text
This is AI Lead Rescue Kit. It helps small businesses capture new leads, qualify them with AI, and save everything into Google Sheets so follow-ups do not get lost.
```

## Show the problem

```text
Many small businesses get leads from forms, emails, and messages, but the follow-up process is manual. Someone has to copy the message into a spreadsheet, decide if the lead is serious, and write a reply.
```

## Show the workflow

```text
Here is the n8n workflow.

A new lead comes in through a webhook.
The workflow normalizes the lead data.
OpenAI summarizes and qualifies the lead.
Then the result is added to Google Sheets.
```

## Run sample lead

Use this sample:

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

## Show Google Sheet result

Point out:

```text
Lead Type: Hot
Urgency: High
Need Summary
Suggested Reply
Next Action
Follow-up Date
Status
```

## Explain safety

```text
The workflow does not automatically send emails. It creates a complete email draft in Suggested Reply, including a subject line, greeting, body, next-step question, and signature placeholder, so a human can review and send it.
```

## Close

```text
You can use this as a lightweight CRM before moving to a larger system. It is best for small teams that already use Google Sheets and want to stop missing inbound leads.
```

## CTA

```text
Download the workflow, connect your OpenAI key and Google Sheet, then test it with your own lead form.
```
