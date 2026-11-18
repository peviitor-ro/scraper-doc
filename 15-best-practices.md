# 15 — Best Practices and Common Pitfalls

Learn from mistakes others have made. Keep this list handy while building your scraper. For schemas, always check [peviitor_core](https://github.com/peviitor-ro/peviitor_core#job-model-schema).

## URL hygiene

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Unescaped `&` in job URL | The upload API runs `htmlspecialchars()`, turning `&` into `&amp;` and breaking the link | Strip tracking params; skip if a real `&` remains |
| Using `inviitor.ro/job/<id>` deep link | These deep links can return "Job not found" on the employer site | Store the real employer job URL, not an aggregator link |
| Tracking host (bit.ly, t.co, jobviewtrack.com) | Redirects may die or hide the real job | Follow redirects; skip tracking URLs |
| Generic careers page (no job ID) | Not a specific job; just the careers homepage | Detect generic paths and skip |
| Duplicate URL | Same job pushed twice | Check against `data/uploaded_urls.txt` before pushing |

## Company and CIF/CUI

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Unknown company | Job can be linked to the wrong company or skipped later by validation | Verify CIF/CUI and create the company first; see [10](10-company-creation.md) |
| Wrong CIF digits | Job linked to the wrong company | Copy the value from a verified source; never invent or reformat it |
| `RO` prefix on CIF | Invalid format | Use digits only |
| Wrong company name | Job linked to wrong company | Use the exact legal name from the Trade Register, uppercase, with diacritics |

## Data quality

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Placeholder title (`N/A`, `-`, `NESPECIFICAT`) | Low-quality data; may be rejected or harm search | Validate title length ≥ 3 chars and reject placeholders |
| Invented optional fields | Fake data hurts search quality | Only use fields the source actually provides |
| Fabricated tags | Fake data hurts search quality | Use only tags from the source; lowercase, no diacritics, max 20 |
| Expired jobs | Clutters search with unavailable jobs | Use `expirationdate` when the source provides it; skip past dates |
| Copying job descriptions | Unnecessary data and GDPR risk | Store title + URL only; see [16](16-gdpr-data-minimization.md) |

## Process

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| No state file | Cannot resume after interruption; reprocesses everything | Save `last_run` or `last_page` after each successful batch |
| No audit trail | Cannot prove what was uploaded | Append accepted URLs to `data/uploaded_urls.txt` |
| Not verifying after push | You don't know if jobs actually appeared | Search the API after every push; see [13](13-testing-verification.md) |
| Multiple new scrapers in one repo | Harder to assign ownership and isolate failures | Prefer one scraper per repo for new work |
| Scraper in a personal repo | Hard to maintain if the volunteer leaves | Create new scrapers in [peviitor-scrapers](https://github.com/peviitor-scrapers); transfer active ones to the org |

## AI usage

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Trusting AI code without testing | AI can generate plausible but wrong code | Always run and verify: push 5 jobs, check they appear in peviitor |
| Not explaining errors to AI | AI can't fix what it doesn't understand | Paste the full error message and your code when asking for help |

## GDPR and data minimization

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Copying job description text | Not needed; peviitor stores title + link | Extract only title + URL; link points to the employer's page |
| Capturing CVs or applications | GDPR violation; peviitor is not an ATS | Never capture, receive, or store CVs |
| Collecting candidate personal data | GDPR violation | Never collect names, emails, phones, or addresses of candidates |
| Storing more than needed | Unnecessary data increases risk | Data minimization: title + link first, optional non-personal fields only |

## Golden rules

1. **For new scrapers, create one repo per scraper in [peviitor-scrapers](https://github.com/peviitor-scrapers).**
2. **Company should exist in peviitor core, with verified CIF/CUI and legal name, before pushing jobs.**
3. **URL must point to the real employer job page — no tracking params and no unescaped `&`.**
4. **Verify after every push — search peviitor to confirm your jobs are there.**
5. **Save state and accepted URLs after each batch so you can resume and audit.**
6. **Look up CIF/CUI on [cuiscan.ro](https://cuiscan.ro) or [cuifirma.ro](https://cuifirma.ro) — never guess.**
7. **GDPR: capture title + link, plus optional non-personal fields only. No descriptions, CVs, or personal data.**
