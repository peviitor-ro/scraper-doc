# 08 — Scraper Architecture

## Where scrapers live

New scrapers are created in the [**peviitor-scrapers**](https://github.com/peviitor-scrapers) organization. The repo belongs to the organization, not to the volunteer who wrote it, so the scraper can be maintained if that volunteer steps away.

For new work, we recommend **one scraper = one git repository** inside that organization. Some older repos live in personal accounts or group multiple scrapers; those are historical.

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
- A volunteer can maintain one repo end to end, while the organization keeps ownership.

## Repository structure

The layout is the same regardless of language. Use the dependency and entrypoint files your ecosystem expects — for example `package.json`, `composer.json`, `Cargo.toml`, `go.mod`, or a `Makefile` — but keep the same responsibilities.

```text
my-company-scraper/
├── README.md                 # What this scraper does and how to run it
├── LICENSE
├── src/                      # Scraper code
│   ├── entrypoint            # e.g. main.ts, index.js, index.php, main.c, main.rs
│   ├── config                # Company identity and source URLs
│   ├── client                # Fetches the careers page or jobs API
│   ├── parser                # Extracts jobs from HTML, JSON, or XML
│   ├── validator             # Checks fields against the job model
│   ├── api                   # Uploads jobs to peviitor
│   └── state                 # Saves progress and the audit trail
├── tests/                    # Automated checks
├── .github/workflows/        # Scheduled run and verification
├── data/                     # Local working state, usually gitignored
│   ├── state.json            # Last page processed, for resuming
│   └── uploaded_urls.txt     # Audit trail of uploaded job URLs
└── .gitignore
```

File names and extensions are up to you; the responsibilities are not. The same design works in TypeScript, PHP, JavaScript, C, Rust, or any other language.

`data/` is local working state. Add it to `.gitignore` unless you intentionally want to commit the audit trail.

## What the scraper does

Your scraper should:

1. Read the company's careers page or call its jobs API.
2. For each job, extract the required `title` and `url`, plus only optional non-personal fields that are clearly available.
3. Validate each job against the [job model](https://github.com/peviitor-ro/peviitor_core#job-model-schema) and [best practices](15-best-practices.md).
4. Push valid jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`.
5. Save progress so you can resume if interrupted.
6. Append uploaded URLs to `data/uploaded_urls.txt` for audit.

See [18 — Components of a scraper repository](18-scraper-components.md) for a concrete implementation of these parts.

## Assigning a scraper

When a new volunteer joins:

1. Create a new repo in [peviitor-scrapers](https://github.com/peviitor-scrapers), such as `company-name-scraper`.
2. The volunteer maintains that scraper: build, test, push jobs, and fix breakages.
3. They verify the jobs and keep the audit trail in `data/uploaded_urls.txt`.
4. The repo stays in the organization when the volunteer leaves, so another maintainer can take over.
