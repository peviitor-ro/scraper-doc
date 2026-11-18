# 16 — GDPR and Data Minimization

## Core principle

**We do not process or store any GDPR-related data.**

A scraper for peviitor.ro must collect the absolute minimum data needed to link a job to its source:

| Field | Captured? | Reason |
|-------|-----------|--------|
| Job title | Yes | Required to identify the position |
| Link to job description | Yes | Required to send candidates to the employer's page |
| Job description text | No | Never copied or stored under the peviitor brand |
| Company name | Yes | Required to link the job to a company; legal name from [cuiscan.ro](https://cuiscan.ro) / [cuifirma.ro](https://cuifirma.ro) |
| CIF/CUI | Yes | Required company identifier |
| Location (city/county) | Optional | Only if available on the careers page |
| Work mode (remote/on-site/hybrid) | Optional | Only if clearly stated |
| Salary | Optional | Only if clearly stated |
| CVs | No | Never captured or stored |
| Candidate personal data | No | Never captured or stored |
| Applicant data | No | Never captured or stored |

## Rules for every scraper

1. **Title and link are the baseline.** The scraper extracts the job title and the canonical URL to the employer's job description page.
2. **No job description copying.** Do not scrape, copy, or store the full job description text. The link points candidates to the employer's own page.
3. **Optional fields must be non-personal and verified.** Location, work mode, and salary are allowed only when the source states them clearly.
4. **No CVs.** The scraper never asks for, receives, or stores CVs or applications.
5. **No personal data.** The scraper never collects candidate names, emails, phone numbers, addresses, or other personally identifiable information.
6. **No GDPR data at all.** If a field could be considered personal data under GDPR, do not capture it.

## What peviitor stores

Following the [job model](https://github.com/peviitor-ro/peviitor_core#job-model-schema), peviitor stores:

- Job title and job URL.
- Company name and CIF/CUI, for grouping.
- Optional non-personal fields: location, work mode, salary, tags, and dates.

It does **not** store candidate data, CVs, or application data.

## If you are unsure

When in doubt, leave it out. It is better to push fewer fields than to accidentally capture GDPR-protected data.

Ask your coordinator if a field is unclear. For implementation details, see [12 — Scraper template](12-scraper-template.md) and [15 — Best practices](15-best-practices.md).
