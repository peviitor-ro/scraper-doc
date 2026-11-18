# 08 — Scraper Architecture: One Scraper, One Repository

## The rule

**One scraper = one git repository.**

If you are building scrapers for 10 companies, you create 10 repositories.

## Why?

Romania has over 1 million companies. Each career page is different:
- Company A uses a WordPress jobs page.
- Company B uses a Greenhouse API.
- Company C uses a Workday portal.
- Company D has no API, only HTML.

If all scrapers live in one repo, a change to Company A's page could break Company B's scraper. Separate repos mean:
- Each scraper is independent.
- You can deploy, fix, or delete one without touching others.
- Each repo has its own history, issues, and tests.
- Volunteers can own one repo end-to-end.

## Repository structure

```
my-company-scraper/
├── README.md                 # What this scraper does
├── scraper.py                # Main scraper code
├── requirements.txt          # Python dependencies (if any)
├── data/
│   └── state.json            # Last page processed, for resuming
├── uploaded_urls.txt         # Audit trail of uploaded job URLs
└── .gitignore                # Ignore data/, __pycache__/
```

## What goes in scraper.py

Your scraper should:
1. Read the company's career page or call its jobs API.
2. For each job: extract title, company, CIF/CUI, location, URL, salary, workmode.
3. Validate each job against peviitor rules (see 02-data-model.md and 04-company-model.md).
4. Push valid jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`.
5. Save progress so you can resume if interrupted.
6. Record uploaded URLs for audit.

## Repository per volunteer

When a new volunteer joins, assign them one company:
1. Create a new repo: `company-name-scraper`.
2. The volunteer builds and owns that scraper.
3. They push jobs, verify them, and maintain the scraper over time.
