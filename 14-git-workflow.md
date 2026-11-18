# 14 — Git Workflow

For new work, each company scraper lives in its own git repository. This keeps scrapers independent and easy to maintain. Older multi-scraper repos still exist; they are historical and are not the model for new scrapers. See [08 — Scraper architecture](08-scraper-architecture.md).

## Creating a new scraper repo

```bash
# 1. Create the folder (replace ~/scrapers with your own path)
mkdir -p ~/scrapers/<company-name>-scraper
cd ~/scrapers/<company-name>-scraper

# 2. Initialize git
git init

# 3. Create initial files
echo "# <Company Name> Scraper" > README.md
echo "data/" > .gitignore
echo "__pycache__/" >> .gitignore

# 4. Add and commit
git add .
git commit -m "Initial scraper structure for <Company Name>"

# 5. Push to your remote (GitHub, GitLab, etc.)
git remote add origin <your-repo-url>
git push -u origin main
```

## Daily work loop

```bash
# 1. Pull latest changes (if collaborating)
git pull

# 2. Work on the scraper
# ... edit scraper.py ...

# 3. Test locally (DRY_RUN = True first)
python3 scraper.py

# 4. Commit changes
git add scraper.py
git commit -m "Fix URL extraction for <Company Name>"

# 5. Push
git push
```

## Commit message conventions

Use clear, short messages:

- `Initial scraper structure for <Company>`
- `Add job URL extraction`
- `Fix CIF lookup for <Company>`
- `Push jobs to peviitor API`
- `Add resume state support`

Avoid vague messages like `fix` or `update`.

## Ownership

When a new volunteer joins:

1. Assign them one company.
2. They create, or are given, one repo for that scraper.
3. They own it: build, test, push, and maintain.

If a volunteer leaves, the repo remains with the scraper code and history.

## New work: prefer one repo per scraper

```text
❌ AVOID for new scrapers:
scrapers/
├── scraper_a.py
├── scraper_b.py
└── scraper_c.py

✅ PREFERRED for new scrapers:
scraper-a/ is its own repo
scraper-b/ is its own repo
scraper-c/ is its own repo
```

If you are updating an older multi-scraper repo, keep its existing structure unless the team agrees to split it.
