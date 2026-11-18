# 13 — Testing and Verification

After pushing jobs to peviitor, you must verify they actually appear in the search engine. Do not assume success just because the API returned `200 OK`.

## Step 1: Check the upload response

When you POST jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`, a successful response looks like:

```json
{
  "success": "Jobs successfully uploaded to Solr",
  "count": 5
}
```

`count` is the number of jobs accepted. If it is `0` or lower than expected, something was skipped. Common error responses are `400` for an invalid payload and `503` if the search core is unavailable. The implementation is in the [API repository](https://github.com/peviitor-ro/api).

## Step 2: Search peviitor for your jobs

Use the search API to find your company's jobs:

```bash
# Search by company name
curl "https://api.peviitor.ro/v1/search/?q=EXAMPLE%20COMPANY&rows=50&page=1"

# Search by a specific job title
curl "https://api.peviitor.ro/v1/search/?q=JOB%20TITLE&rows=10&page=1"
```

Look in `response.docs[]` for:

- Your job URLs.
- Your company name and CIF/CUI.
- Any optional fields you expected, such as location.

## Step 3: Verify by URL

If you know a specific job URL you pushed, search for a distinctive part of it and check the results:

```bash
curl "https://api.peviitor.ro/v1/search/?q=job-id-12345&rows=10&page=1"
```

If the job appears, the push worked.

## Step 4: Check for duplicates or skips

If you run the scraper again, jobs already in peviitor should be skipped rather than duplicated. Check your audit file, `data/uploaded_urls.txt`, to see what was uploaded. The template in [12 — Scraper template](12-scraper-template.md) writes this file automatically.

Location is optional in the [job model](https://github.com/peviitor-ro/peviitor_core#job-model-schema); a missing location does not mean the job was skipped. Expiry, when provided, uses the `expirationdate` field.

## Step 5: Common verification failures

| Symptom | Likely cause |
|---------|--------------|
| Job not found in search | Push failed, URL is wrong, or indexing has not completed yet |
| Job found but wrong company | CIF/CUI does not match the intended company in core |
| Job found but missing optional fields | Source did not provide them; do not invent values |
| Duplicate jobs | Scraper ran twice without a dedup guard |
| `400` from upload endpoint | Payload was not a non-empty JSON array, or no job had `url`, `title`, and `company` |

## Step 6: Record the result

Update `data/state.json` with:

- Last page or date processed.
- Number of jobs fetched and uploaded.
- Any errors encountered.

This helps you resume later without re-pushing old jobs.
