# 10 — Creating a Company in Peviitor Core

## When to create a company

Before pushing jobs, the company should exist in the [peviitor company core](https://github.com/peviitor-ro/peviitor_core#company-model-schema). The mandatory fields are:

- `id` — the CIF/CUI (digits only, no `RO` prefix).
- `company` — the legal name in uppercase, with diacritics where applicable.

If the company already exists, note its CIF and use it when pushing jobs. If it does not exist, create it first. See [09 — Source of truth](09-source-of-truth.md) for how to verify the values.

## API endpoint

```text
PUT https://api.peviitor.ro/v1/firme/company/add/
```

Body (JSON):

```json
{
  "id": "12345678",
  "company": "EXAMPLE COMPANY S.R.L.",
  "brand": "Example",
  "status": "activ",
  "career": ["https://example.com/careers"],
  "website": ["https://example.com"],
  "scraperFile": "example-scraper.py",
  "lastScraped": "2026-11-18"
}
```

Only `id` and `company` are required. The rest are optional but recommended. The API currently accepts 2–10 digits for `id`, and `status` must be one of `activ`, `suspendat`, `inactiv`, or `radiat`. The endpoint is an upsert, so calling it for an existing CIF updates that company rather than failing. The implementation is in the [API repository](https://github.com/peviitor-ro/api).

## Step-by-step

1. Look up the company on [cuiscan.ro](https://cuiscan.ro) / [cuifirma.ro](https://cuifirma.ro).
2. Confirm the CIF/CUI and legal name.
3. Call `PUT /v1/firme/company/add/` with at least `{id, company}`.
4. If the API says the company already exists, note the CIF and move on.
5. Use that CIF as the `cif` field when pushing jobs.

## Verification

After creating the company, confirm it exists:

```text
GET https://api.peviitor.ro/v1/firme/company/?name=EXAMPLE%20COMPANY%20S.R.L.
```

You can also search by CIF with `?cif=12345678`. If the response contains the company with the expected CIF, you are ready to push jobs. See [13 — Testing and verification](13-testing-verification.md).
