# 14 — Git Workflow: One Repo Per Scraper

Each company scraper lives in its own git repository. This keeps scrapers independent and easy to maintain.

## Creating a new scraper repo

```bash
# 1. Create the folder
mkdir /home/sebi/scrapers/<company-name>-scraper
cd /home/sebi/scrapers/<company-name>-scraper

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

# 3. Test locally
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

## One repo per volunteer

When a new volunteer joins:
1. Assign them one company.
2. They create (or are given) one repo for that scraper.
3. They own it: build, test, push, maintain.

If a volunteer leaves, the repo remains with the scraper code and history.

## Do NOT put multiple scrapers in one repo

```
❌ WRONG: scrapers/ contains scraper_a.py, scraper_b.py, scraper_c.py
✅ RIGHT: scraper-a/ is its own repo, scraper-b/ is its own repo
```
