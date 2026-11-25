# 20 — Performance and Reliability

> Standalone chapter. It is intentionally a new file: the numbered files `00`–`19` and `README.md` are unchanged. Add it to the documentation index in a separate change.

A scraper is "fast enough" when it finishes well inside its schedule, does not get blocked by the source, and produces data you can trust. Most slow scrapers are slow because of **network round trips**, not because of the language they are written in. Optimise requests first, CPU second.

## 1. Measure before optimising

Record these numbers on every meaningful change:

| Metric | What to log | Example from `db-global-technology-srl-python-scraper` |
|--------|-------------|--------------------------------------------------------|
| Source requests | number of HTTP calls to the careers page/API | 6 search pages (20 jobs each) for 101 jobs |
| Fetch time | time from first to last source request | ~9.7 s including deliberate pauses |
| Upload calls | number of POSTs to the peviitor API | 1 POST for all jobs, 1 POST for the test batch |
| Verification time | search calls + retries until jobs are found | ~5 s (3 search pages + one indexing retry) |
| Total wall clock | full run | 9.7 s dry run / 16.5 s for 96 jobs + verification |

Without these numbers, "it feels slow" cannot be acted on. Put them in `data/state.json` (`fetched`, `uploaded`, `last_run`, and optionally `duration_seconds`).

## 2. Source-side etiquette (also the biggest speed lever)

1. **Prefer a JSON/API source over scraping HTML.** One JSON call can replace dozens of page loads. Parse HTML only when there is no API.
2. **Find the real page size.** Do not assume `limit=100` works. The Workday CXS endpoint used by DB Global Technology returns `400 Bad Request` for `limit=50` or `100`, and caps pages at 20 — so 101 jobs need 6 requests. Test the limit once and document it in your scraper README.
3. **Use a stable, identifiable `User-Agent`.** Anonymous default clients are more likely to be challenged or rejected. The peviitor API itself returns `403` when no User-Agent is sent.
4. **Pause between pages** (start with ~1 s). Cloudflare/WAF protection on career sites returns `429` when hammered; a challenge then costs 2+4+8+16 s of backoff, far more than the pauses saved.
5. **Retry only transient errors.** Retry `429` and `5xx` with exponential backoff and respect `Retry-After`. Fail immediately on `400`, `401`, `403`, `404`, `405`, `415`, `422`: retrying a bad payload just wastes time.
6. **Do not fetch job detail pages just to get the title.** Workday search results already contain `title` and `externalPath`; the canonical employer URL can be built locally. Fetching 101 detail pages would add ~101 round trips and would pull job descriptions you are not allowed to store (see [16](16-gdpr-data-minimization.md)).
7. **Avoid parallel page fetching unless the source owner allows it.** Concurrency is the quickest way to turn a 10 s run into a rate-limited failure.

### Workday pagination gotcha

On the DB Global Technology endpoint, `total` is only reported reliably on the **first** page (`offset=0`); later pages return `total: 0` while still returning jobs. Keep the first-page total, stop when `offset >= total`, deduplicate by URL, and stop immediately on an empty page. Do not restart the count from each response.

## 3. Upload and verification

- **One bulk POST** with a JSON array (or `{"jobs": [...]}`), not one POST per job. See [03](03-push-and-verify.md) and the [API documentation](https://github.com/peviitor-ro/api).
- Check the response: `{"success": "...", "count": N}`. A `count` lower than expected means some jobs were skipped; log it.
- **Verification has an indexing lag.** The upload endpoint uses Solr `commitWithin`, so an immediate search can return 0 hits even though the upload succeeded. Retry the search 2–3 times with a few seconds between attempts before declaring failure.
- Search with `q=<company name>`, `rows=50`, `page=1..n`, and compare **`response.docs[].url`** against your audit file. `numFound` can be much larger than the number of your jobs because the query also matches other companies.
- Keep `data/uploaded_urls.txt` and skip URLs already in it before every upload. This is both deduplication and your audit trail.

### Verification example

```text
POST /v1/scraper/jobs/upload/  -> {"count": 96}
GET  /v1/search/?q=DB%20GLOBAL%20TECHNOLOGY&rows=50&page=1   -> 0 of 96 found
sleep 5s
GET  /v1/search/?q=DB%20GLOBAL%20TECHNOLOGY&rows=50&page=1..3 -> 96 of 96 found
```

Do not report success on the first `200`, and do not report failure on the first empty search.

## 4. Incremental runs (optional, with a trade-off)

If the source sorts jobs newest-first, you can stop early once a whole page of URLs is already in `uploaded_urls.txt`:

```text
fetch page 1
if every URL in page 1 was uploaded before:
    stop -> 1 source request instead of 6
else:
    continue paginating
```

This reduces a daily dry run to one request. The trade-off: jobs removed from the source site will no longer be detected. If you use it, keep a separate full scan (weekly) that also performs stale-job cleanup — see the EPAM flow described in [18](18-scraper-components.md). Make this an explicit flag (for example `--stop-on-seen`), never the silent default.

## 5. Reliability checklist

- [ ] Source requests and their latency are logged in `data/state.json`.
- [ ] Pagination limit is verified against the real source, not guessed.
- [ ] Stable User-Agent, polite delay, and backoff for `429`/`5xx`.
- [ ] No detail-page fetches and no description/CV/personal data stored.
- [ ] URLs are validated: canonical employer host, no tracking params, no unescaped `&` (see [15](15-best-practices.md)).
- [ ] Bulk upload in one POST; `count` checked against the batch size.
- [ ] Post-upload verification retries to cover indexing lag.
- [ ] `data/uploaded_urls.txt` written only for URLs accepted by the API.
- [ ] Scheduled workflow has a dry-run default; production upload is opt-in.
- [ ] A failed run can resume without re-pushing everything.

## 6. Anti-patterns

| Anti-pattern | Cost |
|--------------|------|
| Fetching every job detail page | 100× more requests, plus GDPR risk |
| One API POST per job | 100× more API calls and failure points |
| `limit=100` assumed without testing | `400` and a broken scraper (Workday caps at 20) |
| No delay between pages | Cloudflare `429`, then long backoff |
| Verifying immediately after upload | False "jobs missing" report due to Solr lag |
| Treating `numFound` as your job count | Wrong numbers; query matches other companies |
| Parallel page fetches | Rate limiting and non-deterministic ordering |

## Reference implementation

[`peviitor-scrapers/db-global-technology-srl-python-scraper`](https://github.com/peviitor-scrapers/db-global-technology-srl-python-scraper) implements this chapter: search-only fetching, 1 s page delay, `429`/`5xx` backoff, one bulk upload, audit-based deduplication, and verification with retries.
