# AI Side Job Radar Setup

## Current MVP

This MVP handles one job post at a time:

```text
Manual job input
-> Fetch job page from jobLink
-> Clean page text
-> OpenAI opportunity analysis
-> Score calculation
-> Append row to Google Sheet
```

It intentionally does not scrape job platforms yet. The first goal is to validate recommendation quality.

## Required Google Sheet

Sheet name:

```text
AI Side Job Radar
```

Tab:

```text
Opportunities
```

Header row:

```text
Date Added
Source
Job Title
Job Link
Recruiter Contact
Raw Requirement
Full Job Requirement
Job Category
Real Need
Deliverable
Required Tools
Required Skills
Your Match Score
AI Leverage Score
Async Fit Score
Learning Value
Delivery Risk
Opportunity Score
Recommended Skillset
Learning Resource
Learning Resource Links
Knowledge Base Topic
Best Practice
Suggested Pitch
Next Action
Status
Notes
```

For normal use, you only need to fill `jobLink`. The workflow tries to fetch and clean the job page automatically.

`rawRequirement` is optional. Use it only when:

- the job platform blocks automated fetching
- the link requires login
- the fetched text is incomplete
- you want to test with pasted job text

Recruiter contact details are extracted only when they appear in the fetched or pasted job text.

## n8n Import

Import this workflow:

```text
workflows/ai-side-job-radar-manual-ingest.n8n.json
```

Then configure two nodes.

### 1. Analyze With OpenAI

Replace:

```text
REPLACE_WITH_OPENAI_API_KEY
```

with your OpenAI API key in n8n. Keep the `Bearer ` prefix in the Authorization header.

The default model is:

```text
gpt-4o-mini
```

If your OpenAI account does not have access to that model, replace the model value in the HTTP body with any chat model available to your account.

### 2. Append To Google Sheet

Replace:

```text
REPLACE_WITH_GOOGLE_SHEET_ID
```

with the ID from your Google Sheet URL.

Example:

```text
https://docs.google.com/spreadsheets/d/SHEET_ID_HERE/edit
```

Also connect your Google Sheets credential in n8n.

## Manual Test

1. Open the `Set Job Input` node.
2. Replace:
   - `source`
   - `jobTitle`
   - `jobLink`
3. Leave `rawRequirement` blank for normal tests.
4. Run the workflow.
5. Check the `Opportunities` tab.

## Recommended First Test Job

Use a narrow automation job, for example:

```text
Source: Upwork
Job Title: Zapier Automation Workflow Using Clickfunnel & Synthflow AI
Job Link: https://www.upwork.com/freelance-jobs/apply/Zapier-Automation-Workflow-Using-Clickfunnel-Synthflow_~021889529082921782234/
Raw Requirement: leave blank first. If Upwork blocks fetching, paste the job post text here as fallback.
```

## Decision Rule

Shortlist opportunities when:

```text
Your Match Score >= 6
AI Leverage Score >= 7
Async Fit Score >= 8
Delivery Risk <= 5
```

Archive opportunities when:

```text
Delivery Risk >= 8
```

or when they require:

```text
full-stack app development
machine learning model training
heavy meetings
production infrastructure ownership
unclear enterprise transformation
```

## Next Upgrade

After 10 manual runs, inspect which sources produce useful recommendations. Then add one sourcing workflow at a time:

1. n8n Community Jobs search
2. Upwork search via a scraper/API provider
3. Freelancer search
4. Daily email digest
