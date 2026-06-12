# Handoff Checklist

Use this checklist when delivering the setup to a client.

## Access

- [ ] Client has n8n access.
- [ ] Client has Google Sheet access.
- [ ] Client has OpenAI API key.
- [ ] Client understands where credentials are stored.

## Google Sheet

- [ ] `Leads` tab exists.
- [ ] Header row matches `lead-tracker-schema.csv`.
- [ ] Test lead row was added successfully.
- [ ] Client knows how to filter by `Lead Type`, `Urgency`, and `Status`.

## n8n

- [ ] Workflow imports successfully.
- [ ] OpenAI node is configured.
- [ ] Google Sheets node is configured.
- [ ] Manual Trigger test passes.
- [ ] Webhook test passes.
- [ ] Workflow is activated.
- [ ] Production webhook URL is connected to the client form.

## AI output

- [ ] Hot lead classification makes sense.
- [ ] Warm lead classification makes sense.
- [ ] Spam handling makes sense.
- [ ] Suggested reply sounds like the client.
- [ ] Follow-up dates are reasonable.

## Client handoff

- [ ] Send the client the Google Sheet link.
- [ ] Send the production webhook URL only if they need it.
- [ ] Send a short Loom walkthrough.
- [ ] Explain that replies are drafts, not auto-sent emails.
- [ ] Explain how to edit the prompt safely.

## Recommended Loom walkthrough

```text
1. Show the form submission.
2. Show the n8n execution.
3. Show the new Google Sheet row.
4. Explain Lead Type and Next Action.
5. Show where the suggested reply appears.
```
