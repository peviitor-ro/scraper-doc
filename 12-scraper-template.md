# 12 — Scraper Template

Copy this structure when starting a new company scraper. Fill in the parts marked `# TODO`.

```
my-company-scraper/
├── README.md
├── scraper.py
├── requirements.txt
├── data/
│   └── state.json
├── uploaded_urls.txt
└── .gitignore
```

## scraper.py — minimal template

```python
#!/usr/bin/env python3
"""Scraper for <COMPANY NAME> jobs -> peviitor.ro

GDPR-compliant: captures ONLY job title and link to job description.
No job description text, no CVs, no personal data.
"""

import json
import urllib.request
import urllib.error
import time
from pathlib import Path

# ---------------------------------------------------------------------------
# CONFIGURATION — fill these in
# ---------------------------------------------------------------------------

COMPANY_CIF = "12345678"          # from cuiscan.ro / cuifirma.ro
COMPANY_NAME = "MY COMPANY SRL"   # legal name, uppercase
SOURCE_URL = "https://example.com/careers"   # company career page
API_BASE = "https://api.peviitor.ro/v1"
UA = {"User-Agent": "my-company-scraper", "Content-Type": "application/json"}

DATA_DIR = Path(__file__).resolve().parent / "data"
DATA_DIR.mkdir(exist_ok=True)
STATE_FILE = DATA_DIR / "state.json"
UPLOADED_FILE = DATA_DIR / "uploaded_urls.txt"


def load_json(path, default):
    try:
        return json.loads(path.read_text())
    except Exception:
        return default


def save_json(path, obj):
    path.write_text(json.dumps(obj, ensure_ascii=False, indent=0))


def http_json(method, url, body=None, timeout=60, attempts=3):
    last = None
    for attempt in range(attempts):
        try:
            data = json.dumps(body).encode() if body is not None else None
            req = urllib.request.Request(url, data=data, headers=UA, method=method)
            with urllib.request.urlopen(req, timeout=timeout) as resp:
                return json.loads(resp.read().decode())
        except urllib.error.HTTPError as e:
            last = e
            if e.code == 404:
                raise
            time.sleep(2 ** attempt)
        except Exception as e:
            last = e
            time.sleep(2 ** attempt)
    raise last


def fetch_jobs():
    # TODO: Scrape the company career page at SOURCE_URL.
    # For each job, extract ONLY:
    #   - title: the job position title
    #   - url: the canonical link to the job description on the employer's site
    #
    # DO NOT capture: job description text, CVs, candidate personal data.
    # Return a list of dicts: [{"title": "...", "url": "..."}, ...]
    return []


def build_peviitor_job(raw_job):
    # TODO: Convert raw_job to peviitor job format.
    # Required fields: url, title.
    # Company is linked by CIF/CUI (company must exist in peviitor core first).
    # GDPR-compliant: we do NOT store job description, CVs, or personal data.
    return {
        "url": raw_job.get("url"),
        "title": raw_job.get("title"),
        "company": COMPANY_NAME,
        "cif": COMPANY_CIF,
    }


def upload_jobs(jobs):
    clean = []
    for j in jobs:
        clean.append({k: v for k, v in j.items() if v not in (None, "", [])})
    if not clean:
        return 0
    res = http_json("POST", f"{API_BASE}/scraper/jobs/upload/", clean)
    return res.get("count", len(clean))


def main():
    state = load_json(STATE_FILE, {"last_run": None})
    raw_jobs = fetch_jobs()
    jobs = [build_peviitor_job(j) for j in raw_jobs if j.get("url") and j.get("title")]
    uploaded = upload_jobs(jobs)
    state["last_run"] = "done"
    save_json(STATE_FILE, state)
    print(f"Fetched={len(raw_jobs)} Uploaded={uploaded}")


if __name__ == "__main__":
    main()
```

## requirements.txt

```
# Add any third-party packages you need here.
# The template above uses only Python standard library.
```

## .gitignore

```
data/
__pycache__/
*.pyc
```

## Next steps after copying

1. Replace `COMPANY_CIF` and `COMPANY_NAME` with your real values.
2. Implement `fetch_jobs()` for your source (HTML scraping or API call).
3. Implement `build_peviitor_job()` to map your fields to peviitor fields.
4. Test with a few jobs before running on all.
5. Commit and push to your repo.
