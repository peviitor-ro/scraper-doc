# 15 — Best Practices and Common Pitfalls

Learn from mistakes others have made. Keep this list handy while building your scraper.

## URL hygiene (most common mistakes)

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| `&` in job URL | peviitor upload API runs `htmlspecialchars()`, turning `&` into `&amp;` and breaking the link | Strip tracking params; skip if a real `&` remains |
| Using `inviitor.ro/job/<id>` deep link | These deep links return "Job not found" on the employer site | Always store the real employer job URL, not the aggregator link |
| Tracking host (bit.ly, t.co, jobviewtrack.com) | Redirects may die or hide the real job | Follow redirects; skip unreachable or tracking URLs |
| Generic careers page (no job ID) | Not a specific job; just the careers homepage | Detect generic path segments and skip |
| Duplicate URL | Same job pushed twice | Check against existing peviitor URLs before pushing |

## Company and CIF/CUI

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Unknown company (not in peviitor core) | Job is skipped as `company_not_in_core` | Look up CIF/CUI on cuiscan.ro / cuifirma.ro; create company if missing |
| Zero-padded CIF (`03786280`) | Treated as different from `3786280`; may fail matching | Strip leading zeros |
| `RO` prefix on CIF | Invalid format | Use digits only |
| Wrong company name | Job linked to wrong company | Use exact legal name from Trade Register (uppercase, diacritics) |

## Data quality

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Placeholder title (`N/A`, `-`, `NESPECIFICAT`) | Job is skipped as `bad_title` | Validate title length ≥ 3 chars, not in placeholder list |
| No location | Job is skipped as `missing_location` | Extract city or county from the job page |
| Fabricated tags | Fake data hurts search quality | Only use tags the source actually provides; lowercase, no diacritics, max 20 |
| Pushing expired jobs | Clutters search with unavailable jobs | Check `expires_at`; skip jobs in the past |

## Process

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| No state file | Cannot resume after interruption; reprocesses everything | Save `last_page` or `last_run` after each successful batch |
| No audit trail | Cannot prove what was uploaded | Append uploaded URLs to `data/uploaded_urls.txt` |
| Not verifying after push | You don't know if jobs actually appeared | Search peviitor API after every push |
| Multiple scrapers in one repo | Hard to maintain; breaks are coupled | One scraper = one repo |

## AI usage

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Trusting AI code without testing | AI can generate plausible but wrong code | Always run and verify: push 5 jobs, check they appear in peviitor |
| Not explaining errors to AI | AI can't fix what it doesn't understand | Paste the full error message and your code when asking for help |

## GDPR and data minimization

| Mistake | Why it breaks | Fix |
|---------|---------------|-----|
| Copying job description text | Not needed; peviitor only stores title + link | Extract only title + URL; link points to employer's page |
| Capturing CVs or applications | GDPR violation; peviitor is not an ATS | Never capture, receive, or store CVs |
| Collecting candidate personal data | GDPR violation (names, emails, phones, addresses) | Never collect any personal data |
| Storing more than title + link | Unnecessary data increases risk | Data minimization: title + link only |

## Golden rules

1. **One scraper, one repo.**
2. **Company must exist in peviitor core (CIF/CUI + name) before pushing jobs.**
3. **URL must be the real employer link — no `&`, no tracking hosts, no deep links.**
4. **Verify after every push — search peviitor to confirm your jobs are there.**
5. **Save state after each batch so you can resume.**
6. **Look up CIF/CUI on cuiscan.ro or cuifirma.ro — never guess.**
7. **GDPR: capture only job title + link. No job description, no CVs, no personal data.**
