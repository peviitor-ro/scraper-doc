# 08 — Scraper Architecture

## The convention

For new scrapers, we recommend **one scraper = one git repository**.

Some older organization repos contain multiple scrapers; those are historical. For new work, keeping each scraper in its own repo makes it easier to assign ownership, fix breakages, and maintain history independently.

## Why?

Romania has many companies, and each careers page is different:

- Company A uses a WordPress jobs page.
- Company B uses a Greenhouse API.
- Company C uses a Workday portal.
- Company D has no API, only HTML.

If all scrapers live in one repo, a change to Company A's page can make it harder to find and fix Company B's scraper. Separate repos mean:

- Each scraper is independent.
- You can deploy, fix, or archive one without touching others.
- Each repo has its own history, issues, and tests.
- A volunteer can own one repo end to end.

## Repository structure

```text
my-company-scraper/
├── README.md                 # What this scraper does
├── scraper.py                # Main scraper code
├── requirements.txt          # Python dependencies (if any)
├── data/
│   ├── state.json            # Last page processed, for resuming
│   └── uploaded_urls.txt     # Audit trail of uploaded job URLs
└── .gitignore                # Ignore data/, __pycache__/, etc.
```

`data/` is local working state. Add it to `.gitignore` unless you intentionally want to commit the audit trail.

## What goes in scraper.py

Your scraper should:

1. Read the company's careers page or call its jobs API.
2. For each job, extract the required `title` and `url`, plus only optional non-personal fields that are clearly available.
3. Validate each job against the [job model](https://github.com/peviitor-ro/peviitor_core#job-model-schema) and [best practices](15-best-practices.md).
4. Push valid jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`.
5. Save progress so you can resume if interrupted.
6. Append uploaded URLs to `data/uploaded_urls.txt` for audit.

A starting point is available in [12 — Scraper template](12-scraper-template.md).

## Assigning a scraper

When a new volunteer joins:

1. Create a new repo such as `company-name-scraper`.
2. The volunteer builds and owns that scraper.
3. They push jobs, verify them, and maintain the scraper over time.
