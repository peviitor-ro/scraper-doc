# 18 — Components of a Scraper Repository

Based on the [peviitor-scrapers organization README](https://github.com/peviitor-scrapers/.github/blob/main/profile/README.md) and the [EPAM Node.js reference scraper](https://github.com/sebiboga/epam-systems-international-srl-nodejs-scraper).

A scraper repository has four main parts: **code**, **testing**, **automation**, and **presentation/documentation**.

## Root files

Files at the root of the repo:

| File | Purpose |
|------|---------|
| `README.md` | What the scraper does, how it works, and how to run it |
| `LICENSE` | License, usually MIT |
| `CODE_OF_CONDUCT.md` | Community rules |
| `CONTRIBUTING.md` | How to contribute or derive a new scraper |
| `SECURITY.md` | How to report security issues |
| `CHANGELOG.md` | Version history |

## `scraper/` — the code

This folder contains the code that fetches jobs and publishes them to peviitor.ro through the API.

In the EPAM reference scraper:

| File | Role |
|------|------|
| `scraper/index.js` | Main entry point: fetch, filter, and orchestrate the run |
| `scraper/config/company.js` | Company identity: CIF/CUI, legal name, careers URL |
| `scraper/config/scraper.js` | Scraper settings such as source URL and User-Agent |
| `scraper/api.js` | Uploads jobs to the peviitor API |
| `scraper/anaf.js` | Validates company data via ANAF, with cache and stale-cache fallback |
| `scraper/company.js` | Company core checks and cross-validation |
| `scraper/job-validator.js` | Validates job fields before upload |
| `scraper/validate-jobs.js` | Post-upload verification and stale-job cleanup |
| `scraper/markdown-generator.js` | Generates `docs/jobs.md` |

Python scrapers use the same idea with files such as `scraper/index.py`, `scraper/api.py`, `scraper/anaf.py`, and `scraper/job_validator.py`. See the [Python examples](17-scraper-examples.md).

## `tests/` — automated checks

The `tests/` folder verifies that the scraper works and that the repo stays consistent. Typical groups are:

- **unit tests** — individual functions
- **integration tests** — API and company checks
- **e2e tests** — full scraper run
- **consistency tests** — required files, topics, workflows, and documentation

## `docs/` — presentation and generated output

The scraper has a small web interface published with GitHub Pages:

- `docs/index.html` — presentation page
- `docs/jobs.md` — generated list of current jobs
- `docs/company.json` — company data used by the presentation
- `docs/test-results/` — automated test reports

## `ai/` — AI prompts and maintenance notes

This folder holds prompts and instructions that can be used with opencode, Claude Code, Antigravity, or another agentic AI tool to create, maintain, and verify the scraper.

## GitHub Actions

Workflows in `.github/workflows/` run the scraper and its checks:

| Workflow | Purpose |
|----------|---------|
| `job-seeker-ro-spider.yml` | Daily scrape and upload |
| `automation-testing.yml` | Automated tests and company existence check |
| `job-deep-validate.yml` | Validates job description pages and detects expired jobs |
| `automation-template-sync-check.yml` | Checks for newer versions of the source template |
| `job-recovery-from-disaster.yml` | Restores company data in company core if it is lost from SOLR |
| `pages-build-deployment` | Builds and deploys `docs/` to GitHub Pages |

## Repository About section

The GitHub About section should contain:

- A short project description.
- A link to the scraper presentation page.
- The topics `job-seeker-ro-spider` and `peviitor-ro`.

## EPAM example flow

The [EPAM scraper](https://github.com/sebiboga/epam-systems-international-srl-nodejs-scraper) is the Node.js reference implementation. Its flow is:

1. Fetch jobs from the EPAM Careers API, with pagination and Romania filtering.
2. Add extra ANOFM jobs found by CIF.
3. Validate the company via ANAF, using a 7-day cache and stale-cache fallback.
4. Cross-validate against the peviitor API.
5. Delete stale jobs that are no longer on the source site.
6. Upload jobs and company data to peviitor.
7. Regenerate `docs/jobs.md` for GitHub Pages.

Not every repo has every file, but reference scrapers follow this structure. Use [17 — Scraper examples](17-scraper-examples.md) to compare real implementations.
