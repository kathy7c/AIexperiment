# Troubleshooting

## Google Sheet row is not added

Check:

1. The tab name is exactly `Leads`.
2. The header names match the schema exactly.
3. `REPLACE_WITH_GOOGLE_SHEET_ID` was replaced with the real Sheet ID.
4. Google Sheets credential is connected in n8n.
5. The Google account has edit access to the sheet.

## OpenAI node fails

Check:

1. The Authorization header starts with `Bearer `.
2. The API key is active.
3. Your OpenAI account has billing enabled.
4. The model `gpt-4o-mini` is available.

If needed, replace:

```text
gpt-4o-mini
```

with another available chat model.

## Webhook test works but production webhook does not

In n8n:

1. Activate the workflow.
2. Open `Lead Webhook`.
3. Copy the Production URL, not the Test URL.
4. Update your form tool with the Production URL.

## The lead is classified incorrectly

Adjust the prompt in:

```text
Build OpenAI Prompt
```

Add client-specific rules, for example:

```text
For this business, any inquiry asking for pricing and availability should be Hot.
Messages from vendors should be Spam.
Messages from existing customers should be Not a Lead unless they request a new service.
```

## Suggested reply sounds too generic

Add a tone rule in the prompt:

```text
Use a concise, friendly tone. Mention the business name only if it appears in the lead data. Ask one clear next-step question.
```

## Duplicate leads

The MVP does not deduplicate leads.

Recommended upgrade:

```text
Before appending, look up existing Email + Original Message in Google Sheets.
If found, update Notes instead of creating a new row.
```

## Auto-send replies

Do not auto-send replies in the MVP.

Recommended safe pattern:

```text
Generate draft -> human reviews -> human sends
```

Auto-send can be added later only after testing with real examples.
