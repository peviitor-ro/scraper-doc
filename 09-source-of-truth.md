# 09 — Source of Truth: CIF/CUI and Company Name

## Why a source of truth matters

A job in peviitor must be linked to a company. The company is identified by:
- **CIF/CUI** — the Romanian tax identification number (mandatory).
- **Company name** — the legal name (mandatory).

You cannot guess these. You must look them up in an authoritative source.

## Two sources of truth

### 1. Company identity (CIF/CUI + name)

Use these Romanian sources to find a company's CIF/CUI and legal name:

| Source | URL | What it gives you |
|--------|-----|-------------------|
| cuiscan.ro | https://cuiscan.ro | Search by company name or CUI; returns CUI, name, status, address |
| cuifirma.ro | https://cuifirma.ro | Search by company name or CUI; returns CUI, name, registration data |

### 2. Job data (title + link)

The **company career page** is the source of truth for job listings.
- Scrape jobs **only** from the company's own career page.
- Do **not** scrape third-party job boards or aggregators.
- For each job, capture **only**: job title and link to the job description.

## How to look up a company

1. Go to cuiscan.ro or cuifirma.ro.
2. Search by the company's name (as it appears on their career page).
3. Confirm the result matches (same city, same line of business).
4. Record the **CUI/CIF** (strip any leading zeros, no `RO` prefix).
5. Record the exact **legal name** (use uppercase, keep diacritics).

## Example

Company on career page: "Orange Romania"

Lookup on cuiscan.ro:
- CUI/CIF: 8537404
- Legal name: ORANGE ROMANIA COMMUNICATIONS S.A.
- Status: activ

You would store:
- `cif`: `8537404`
- `company`: `ORANGE ROMANIA COMMUNICATIONS S.A.`

## What if the company is not found?

If you cannot find the company on cuiscan.ro or cuifirma.ro:
- Do not invent a CIF/CUI.
- Do not push jobs for that company yet.
- Ask your coordinator or skip the company.

## CIF/CUI rules

- 8 digits (Romanian companies).
- No `RO` prefix.
- No leading zeros (e.g. `03786280` → `3786280`).
- Must be numeric only.
