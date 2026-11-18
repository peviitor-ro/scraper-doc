# 11 — Building a Scraper: Step by Step

This is a practical tutorial for volunteers. Follow these steps to build your first scraper.

## Prerequisites

- Python 3 installed on your machine.
- Access to the company's careers page or jobs API.
- The company's CIF/CUI and legal name from [cuiscan.ro](https://cuiscan.ro) or [cuifirma.ro](https://cuifirma.ro).
- An AI assistant such as [opencode.ai](https://opencode.ai) or [kilo.ai](https://kilo.ai), if you want help writing code.

Local folder names are up to you; the remote repo must be in [peviitor-scrapers](https://github.com/peviitor-scrapers).

## Step 1: Create the repository

Create an empty repo in the organization named `my-company-scraper`, then clone it locally:

```bash
git clone https://github.com/peviitor-scrapers/my-company-scraper.git
cd my-company-scraper
```

If you start locally first, push to the organization repo as soon as the scraper is ready. Do not leave new scrapers in personal accounts.

Create `README.md` with one line describing what the scraper does.

## Step 2: Look up the company

Use cuiscan.ro or cuifirma.ro to find:

- CIF/CUI, for example `12345678`.
- Legal name, for example `EXAMPLE COMPANY S.R.L.`.

Create the company in peviitor core if it does not exist; see [10 — Creating a company](10-company-creation.md).

## Step 3: Write the scraper

Create `scraper.py`. Your scraper needs to:

1. **Fetch** jobs from the company's careers page (the source of truth for jobs).
2. **Extract** the required `title` and canonical job `url` for each job.
3. **Do not capture** job description text, CVs, candidate personal data, or any GDPR-related information. See [16 — GDPR and data minimization](16-gdpr-data-minimization.md).
4. **Validate**:
   - Title is present and not a placeholder.
   - URL points to the real employer job page, with no tracking params or unescaped `&`. See [15 — Best practices](15-best-practices.md).
   - Company resolves to a verified CIF/CUI.
5. **Push** valid jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`.
6. **Save** progress in `data/state.json` so you can resume if interrupted.
7. **Record** uploaded URLs in `data/uploaded_urls.txt` for audit.

You can start from the template in [12 — Scraper template](12-scraper-template.md).

## Step 4: Test with a dry run

Run your scraper in dry-run mode if you built one, otherwise push only 1–5 jobs first. Check the API response and confirm the count matches what you sent.

## Step 5: Verify jobs appeared in peviitor

Search the API for your company or a specific job title:

```bash
curl "https://api.peviitor.ro/v1/search/?q=EXAMPLE%20COMPANY&rows=50&page=1"
curl "https://api.peviitor.ro/v1/search/?q=JOB%20TITLE&rows=10&page=1"
```

Look in `response.docs[]` for your job URLs, company name, and expected optional fields. See [13 — Testing and verification](13-testing-verification.md).

## Step 6: Commit and push

```bash
git add README.md scraper.py requirements.txt .gitignore
git commit -m "Initial scraper for Example Company"
git push -u origin main
```

Create the remote repo first in peviitor-scrapers if needed; see [14 — Git workflow](14-git-workflow.md).

## Step 7: Maintain

- Re-run the scraper periodically, for example weekly or monthly.
- Fix it when the company's careers page changes.
- Fix it if the peviitor API changes.
- Keep the repo in peviitor-scrapers so someone else can take over if you step away.

## Getting stuck?

Ask your AI assistant:

> "My scraper got error [error message] when pushing to peviitor. Here is my code: [...]. How do I fix it?"

Or ask your coordinator.

If the documentation itself is unclear, open a new [GitHub Issue](https://github.com/peviitor-ro/scraper-doc/issues/new/choose). Include the step number and the exact error or confusing wording. The answer will be added to the relevant doc so the next volunteer has it.
