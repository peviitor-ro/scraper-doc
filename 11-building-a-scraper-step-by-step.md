# 11 — Building a Scraper: Step-by-Step

This is a practical tutorial for volunteers. Follow these steps to build your first scraper.

## Prerequisites

- Python 3 installed on your machine.
- Access to the company's career page or jobs API.
- The company's CIF/CUI and legal name (from cuiscan.ro / cuifirma.ro).
- An AI assistant (opencode.ai or kilo.ai) to help you write code.

## Step 1: Create the repository

```bash
mkdir /home/sebi/scrapers/my-company-scraper
cd /home/sebi/scrapers/my-company-scraper
git init
```

Create `README.md` with one line describing what the scraper does.

## Step 2: Look up the company

Use cuiscan.ro or cuifirma.ro to find:
- CIF/CUI (e.g. `12345678`)
- Legal name (e.g. `MY COMPANY SRL`)

Create the company in peviitor core if it does not exist (see 10-company-creation.md).

## Step 3: Write the scraper

Create `scraper.py`. Your scraper needs to:

1. **Fetch** jobs from the company's career page (source of truth for jobs).
2. **Extract** for each job: **only** the job title and the link to the job description.
3. **Do NOT capture**: job description text, CVs, candidate personal data, or any GDPR-related information (see 16-gdpr-data-minimization.md).
4. **Validate**:
   - Title is present and not a placeholder.
   - URL is a real employer link (no `&`, no tracking hosts, not a generic careers page).
   - Company resolves to a CIF/CUI in peviitor core.
5. **Push** valid jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`.
6. **Save** progress so you can resume if interrupted.
7. **Record** uploaded URLs to an audit file.

Ask your AI assistant to help generate the initial code:

> "Write a Python scraper that reads jobs from <source URL> and pushes them to https://api.peviitor.ro/v1/scraper/jobs/upload/"

## Step 4: Test with a dry run

Run your scraper in dry-run mode (if you built one) or manually push 1–5 jobs first. Check the API response.

## Step 5: Verify jobs appeared in peviitor

After pushing, search peviitor to confirm your jobs are there:
```
GET https://api.peviitor.ro/v1/search/?q=<JOB-TITLE>&rows=5&page=1
```

Or search by your company's jobs:
```
GET https://api.peviitor.ro/v1/search/?q=*:*&rows=50&page=1
```
Then look for your company name and job URLs in the results.

## Step 6: Commit and push

```bash
git add scraper.py README.md
git commit -m "Initial scraper for My Company"
git push
```

## Step 7: Maintain

- Re-run the scraper periodically (weekly or monthly).
- Fix it when the company's career page changes.
- Fix it if peviitor API changes.

## Getting stuck?

Ask your AI assistant:

> "My scraper got error [error message] when pushing to peviitor. Here is my code: [...]. How do I fix it?"

Or ask your coordinator.
