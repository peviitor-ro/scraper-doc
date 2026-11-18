# 13 — Testing and Verification

After pushing jobs to peviitor, you must verify they actually appear in the search engine. Do not assume success just because the API returned `200 OK`.

## Step 1: Check the upload response

When you POST jobs to `https://api.peviitor.ro/v1/scraper/jobs/upload/`, the API returns something like:

```json
{"count": 5}
```

`count` is the number of jobs accepted. If it is `0` or lower than expected, something was skipped.

## Step 2: Search peviitor for your jobs

Use the search API to find your company's jobs:

```bash
# Search by company name
curl "https://api.peviitor.ro/v1/search/?q=<COMPANY-NAME>&rows=50&page=1"

# Search by a specific job title
curl "https://api.peviitor.ro/v1/search/?q=<JOB-TITLE>&rows=10&page=1"
```

Look in `response.docs[]` for:
- Your job URLs.
- Your company name.
- The correct location.

## Step 3: Verify by URL

If you know a specific job URL you pushed:

```bash
curl "https://api.peviitor.ro/v1/search/?q=<YOUR-JOB-URL>&rows=1&page=1"
```

If the job appears, the push worked.

## Step 4: Check for duplicates or skips

If you run the scraper again, jobs already in peviitor should be skipped (not duplicated). Check your audit file (`uploaded_urls.txt`) to see what was uploaded.

## Step 5: Common verification failures

| Symptom | Likely cause |
|---------|--------------|
| Job not found in search | Push failed, or URL is wrong, or company CIF is invalid |
| Job found but wrong company | CIF/CUI does not match peviitor company core |
| Job found but missing fields | Title, location, or salary not extracted properly |
| Duplicate jobs | Scraper ran twice without dedup guard |

## Step 6: Record the result

Update your `data/state.json` with:
- Last page or date processed.
- Number of jobs uploaded.
- Any errors encountered.

This helps you resume later without re-pushing old jobs.
