# 12 — Scraper Template

Copy this structure when starting a new company scraper. Fill in the parts marked `# TODO`.

```text
my-company-scraper/
├── README.md
├── scraper.py
├── requirements.txt
├── data/
│   ├── state.json
│   └── uploaded_urls.txt
└── .gitignore
```

## scraper.py — minimal template

```python
#!/usr/bin/env python3
"""Scraper for <COMPANY NAME> jobs -> peviitor.ro

GDPR-compliant: captures ONLY job title and link to job description,
plus optional non-personal fields that are clearly available.
No job description text, no CVs, no personal data.
"""

import json
import time
import urllib.error
import urllib.request
from datetime import datetime, timezone
from pathlib import Path

# ---------------------------------------------------------------------------
# CONFIGURATION — fill these in
# ---------------------------------------------------------------------------

COMPANY_CIF = "12345678"          # from cuiscan.ro / cuifirma.ro
COMPANY_NAME = "EXAMPLE COMPANY S.R.L."  # legal name, uppercase
SOURCE_URL = "https://example.com/careers"  # company careers page
API_BASE = "https://api.peviitor.ro/v1"
UA = {"User-Agent": "my-company-scraper", "Content-Type": "application/json"}

DRY_RUN = False  # set to True to fetch without uploading

DATA_DIR = Path(__file__).resolve().parent / "data"
DATA_DIR.mkdir(exist_ok=True)
STATE_FILE = DATA_DIR / "state.json"
UPLOADED_FILE = DATA_DIR / "uploaded_urls.txt"


def load_json(path, default):
    try:
        return json.loads(path.read_text(encoding="utf-8"))
    except (OSError, ValueError):
        return default


def save_json(path, obj):
    path.write_text(json.dumps(obj, ensure_ascii=False, indent=2), encoding="utf-8")


def http_json(method, url, body=None, timeout=60, attempts=3):
    last = None
    for attempt in range(attempts):
        try:
            data = json.dumps(body).encode("utf-8") if body is not None else None
            req = urllib.request.Request(url, data=data, headers=UA, method=method)
            with urllib.request.urlopen(req, timeout=timeout) as resp:
                return json.loads(resp.read().decode("utf-8"))
        except urllib.error.HTTPError as e:
            last = e
            if e.code in (400, 401, 403, 404, 415):
                raise
            time.sleep(2 ** attempt)
        except Exception as e:
            last = e
            time.sleep(2 ** attempt)
    raise last


def fetch_jobs():
    # TODO: Scrape the company careers page at SOURCE_URL.
    # For each job, extract:
    #   - title: the job position title
    #   - url: the canonical link to the job description on the employer's site
    #
    # DO NOT capture: job description text, CVs, candidate personal data.
    # Return a list of dicts: [{"title": "...", "url": "..."}, ...]
    return []


def build_peviitor_job(raw_job):
    # TODO: Map raw_job to the peviitor job format.
    # Required by the upload endpoint: url, title, company.
    # cif is strongly recommended. Optional non-personal fields are allowed.
    return {
        "url": raw_job.get("url"),
        "title": raw_job.get("title"),
        "company": COMPANY_NAME,
        "cif": COMPANY_CIF,
    }


def append_uploaded(job_urls):
    """Audit trail: record URLs accepted by the API."""
    with UPLOADED_FILE.open("a", encoding="utf-8") as fh:
        for url in job_urls:
            fh.write(f"{url}\n")


def upload_jobs(jobs):
    clean = [{k: v for k, v in job.items() if v not in (None, "", [])} for job in jobs]
    if not clean:
        return 0

    res = http_json("POST", f"{API_BASE}/scraper/jobs/upload/", clean)
    count = int(res.get("count", len(clean)))
    append_uploaded([job["url"] for job in clean[:count]])
    return count


def main():
    state = load_json(STATE_FILE, {"last_run": None})
    raw_jobs = fetch_jobs()
    jobs = [
        build_peviitor_job(job)
        for job in raw_jobs
        if job.get("url") and job.get("title")
    ]

    uploaded = 0 if DRY_RUN else upload_jobs(jobs)

    state.update(
        {
            "last_run": datetime.now(timezone.utc).isoformat(),
            "fetched": len(raw_jobs),
            "uploaded": uploaded,
            "dry_run": DRY_RUN,
        }
    )
    save_json(STATE_FILE, state)
    print(f"Fetched={len(raw_jobs)} Uploaded={uploaded} DryRun={DRY_RUN}")


if __name__ == "__main__":
    main()
```

## requirements.txt

```text
# Add any third-party packages you need here.
# The template above uses only Python standard library.
```

## .gitignore

```text
data/
__pycache__/
*.pyc
```

## Next steps after copying

1. Replace `COMPANY_CIF` and `COMPANY_NAME` with verified values from [09 — Source of truth](09-source-of-truth.md).
2. Implement `fetch_jobs()` for your source, using [06 — Reference scrapers](06-reference-scrapers.md) as inspiration.
3. Implement `build_peviitor_job()` to map your fields to the [job model](https://github.com/peviitor-ro/peviitor_core#job-model-schema).
4. Set `DRY_RUN = True` first, then test with a few real jobs.
5. Check the results with [13 — Testing and verification](13-testing-verification.md), then commit and push with [14 — Git workflow](14-git-workflow.md).

The template writes accepted URLs to `data/uploaded_urls.txt` automatically. Keep that file as your audit trail.
