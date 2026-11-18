# 10 — Creating a Company in Peviitor Core

## When to create a company

Before pushing a job, the company must exist in peviitor's company core. The mandatory fields are:
- `id` — the CIF/CUI (digits only, no `RO` prefix).
- `company` — the legal name (uppercase, diacritics required).

If the company already exists, you only need its CIF to link the job.
If it does not exist, you must create it first.

## API endpoint

```
PUT https://api.peviitor.ro/v1/firme/company/add/
```

Body (JSON):
```json
{
  "id": "8537404",
  "company": "ORANGE ROMANIA COMMUNICATIONS S.A.",
  "brand": "Orange",
  "status": "activ",
  "career": ["https://www.orange.ro/careers"],
  "website": ["https://www.orange.ro"],
  "scraperFile": "orange-scraper.py",
  "lastScraped": "2026-11-18"
}
```

Only `id` and `company` are required. The rest are optional but recommended.

## Step-by-step

1. Look up the company on cuiscan.ro / cuifirma.ro (see 09-source-of-truth.md).
2. Confirm the CIF/CUI and legal name.
3. Call `PUT /firme/company/add/` with `{id, company}`.
4. If the API says the company already exists, that's fine — note the CIF and move on.
5. Use that CIF as the `cif` field when pushing jobs for this company.

## Verification

After creating the company, you can confirm it exists by searching peviitor:
```
GET https://api.peviitor.ro/v1/firme/company/?name=<COMPANY-NAME>
```

If it returns the company with your CIF, you are ready to push jobs.
