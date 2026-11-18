# 16 — GDPR and Data Minimization

## Core principle

**We do not process or store any GDPR-related data.**

A scraper for peviitor.ro must collect the absolute minimum data needed to link a job to its source:

| Field | Captured? | Reason |
|-------|-----------|--------|
| Job title | ✅ Yes | Required to identify the position |
| Link to job description | ✅ Yes | Required to send candidates to the employer's page |
| Job description text | ❌ No | Never copied or stored under peviitor brand |
| Company name | ✅ Yes | Required to link job to company (legal name from cuiscan.ro / cuifirma.ro) |
| CIF/CUI | ✅ Yes | Required company identifier |
| Location (city/county) | Optional | Only if available on career page |
| Work mode (remote/on-site/hybrid) | Optional | Only if clearly stated |
| Salary | Optional | Only if clearly stated |
| CVs | ❌ No | Never captured, never stored |
| Personal data (names, emails, phones, addresses of candidates) | ❌ No | Never captured, never stored |
| Applicant data | ❌ No | Never captured, never stored |

## Rules for every scraper

1. **Title and link only.** The scraper extracts the job title and the canonical URL to the employer's job description page. Nothing else is required.
2. **No job description copying.** Do not scrape, copy, or store the full job description text. The link points candidates to the employer's own page where the full description lives.
3. **No CVs.** The scraper never asks for, receives, or stores CVs or applications.
4. **No personal data.** The scraper never collects candidate names, emails, phone numbers, addresses, or any other personally identifiable information.
5. **No GDPR data at all.** If a field could be considered personal data under GDPR, do not capture it.

## What peviitor stores

Peviitor stores only:
- Job title
- Job URL (employer's page)
- Company name + CIF/CUI (for grouping)
- Optional: location, workmode, salary

It does **not** store candidate data, CVs, or application data.

## If you are unsure

When in doubt, leave it out. It is better to push fewer fields than to accidentally capture GDPR-protected data.

Ask your coordinator if a field is unclear.
