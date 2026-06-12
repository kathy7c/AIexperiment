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
Starter Fit Score
Can Start Now
Apply Priority
Starter Fit Reason
Missing Proof
First 2-Hour Prep
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

For automatic sourcing, import this workflow:

```text
workflows/ai-side-job-radar-auto-sourcing.n8n.json
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

## Auto Sourcing Workflow

Use this workflow when you want the system to fetch jobs without pasting individual links:

```text
workflows/ai-side-job-radar-auto-sourcing.n8n.json
```

### What it does

```text
Manual Trigger or Daily Schedule
-> Read existing Job Links from Opportunities
-> Fetch n8n Community Jobs RSS and Freelancer RSS feeds
-> Keyword pre-filter for starter-friendly automation jobs
-> Deduplicate against existing Sheet rows
-> OpenAI starter-fit analysis
-> Append scored rows to Opportunities
```

### Current automatic sources

No extra scraping account is required for these:

```text
n8n Community Jobs
Freelancer - n8n automation
Freelancer - Google Sheets automation
Freelancer - Zapier Google Sheets
Freelancer - OpenAI Google Sheets
Freelancer - workflow automation
```

Upwork is not included in the no-account auto workflow because native Upwork RSS is no longer publicly available. Add Upwork later through a service such as Vollna, Apify, or SerpAPI.

### Configure auto sourcing

In `Read Existing Opportunities`:

```text
REPLACE_WITH_GOOGLE_SHEET_ID
```

In `Append To Google Sheet`:

```text
REPLACE_WITH_GOOGLE_SHEET_ID
```

In `Analyze And Format Rows`, replace:

```javascript
const OPENAI_API_KEY = 'REPLACE_WITH_OPENAI_API_KEY';
```

with your OpenAI API key.

The workflow processes at most 12 new candidate jobs per run to control OpenAI cost. Change this value inside `Fetch And Pre-Filter Jobs` if needed:

```javascript
const MAX_JOBS_PER_RUN = 12;
```

### Daily schedule

The workflow includes:

```text
Daily Schedule
```

Default interval:

```text
every 24 hours
```

Keep it inactive while testing. Run `Manual Trigger` first. After the rows look useful, activate the workflow.

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

## Starter Filter

Filter the sheet by:

```text
Can Start Now = Yes
Apply Priority = Apply Now
Status = Shortlisted
```

These are the jobs Kathy can reasonably apply to now.

Use this interpretation:

| Field | Meaning |
| --- | --- |
| Starter Fit Score | 1-10 score for how realistic the job is for Kathy's current stage |
| Can Start Now | `Yes`, `Maybe`, or `No` |
| Apply Priority | `Apply Now`, `Build Demo First`, `Save for Learning`, or `Skip` |
| Missing Proof | What portfolio/demo proof is missing before applying confidently |
| First 2-Hour Prep | One concrete prep task Kathy can do before applying or building a demo |

The workflow marks `Status` automatically:

| Apply Priority | Status |
| --- | --- |
| Apply Now | Shortlisted |
| Build Demo First | Needs Demo |
| Save for Learning | Learning |
| Skip | Archived |

## Decision Rule

Shortlist opportunities when:

```text
Your Match Score >= 6
AI Leverage Score >= 7
Async Fit Score >= 8
Delivery Risk <= 5
Can Start Now = Yes
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
