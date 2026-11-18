# 09 — Source of Truth: CIF/CUI and Company Name

## Why a source of truth matters

A job in peviitor must be linked to a company. The company is identified by:

- **CIF/CUI** — the Romanian tax identification number, stored as `id` in the company core and as `cif` on jobs.
- **Company name** — the legal name, stored as `company`.

Do not guess either value.

## Two sources of truth

### 1. Company identity (CIF/CUI + name)

Use these Romanian sources to find a company's CIF/CUI and legal name:

| Source | URL | What it gives you |
|--------|-----|-------------------|
| CUIscan | [cuiscan.ro](https://cuiscan.ro) | Search by company name or CUI; returns CUI, name, status, address |
| CUIfirma | [cuifirma.ro](https://cuifirma.ro) | Search by company name or CUI; returns CUI, name, registration data |

### 2. Job data (title + link)

The **company careers page** is the source of truth for job listings.

- Scrape jobs only from the company's own careers page.
- Do not scrape third-party job boards or aggregators for job data.
- Required per job: `title` and the canonical `url` of the employer's job page.
- Optional non-personal fields such as `location`, `workmode`, or `salary` are allowed only when clearly stated by the source. See [16 — GDPR and data minimization](16-gdpr-data-minimization.md).

## How to look up a company

1. Go to [cuiscan.ro](https://cuiscan.ro) or [cuifirma.ro](https://cuifirma.ro).
2. Search by the company's name as it appears on its careers page.
3. Confirm the result matches: same city, same line of business, active status.
4. Record the **CUI/CIF** digits and the exact **legal name**.
5. If a source prints a `RO` prefix, drop only that prefix. Otherwise copy the digits exactly; do not invent or reformat the number.

## Example

These values are fictional and are for illustration only:

- Company on careers page: "Example Company"
- Lookup result CUI/CIF: `12345678`
- Lookup result legal name: `EXAMPLE COMPANY S.R.L.`
- Status: `activ`

You would store:

```json
{
  "cif": "12345678",
  "company": "EXAMPLE COMPANY S.R.L."
}
```

## What if the company is not found?

If you cannot find the company on cuiscan.ro or cuifirma.ro:

- Do not invent a CIF/CUI.
- Do not push jobs for that company yet.
- Ask your coordinator or skip the company.

## CIF/CUI rules

- Digits only; no `RO` prefix.
- The current upload/company API accepts 2–10 digits. The core schema commonly uses 8-digit CUIs.
- Copy the value from your verified source exactly; do not change or pad digits.
- Must be numeric only.

If you need to create the company in the core, continue with [10 — Creating a company in peviitor core](10-company-creation.md).
