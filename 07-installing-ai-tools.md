# 07 — Installing and Using AI Tools

You can use [opencode.ai](https://opencode.ai) and [kilo.ai](https://kilo.ai) to help write your scraper. These are AI coding assistants that can generate code, explain errors, and suggest fixes.

## Installing opencode.ai

Follow the installation instructions on the [opencode.ai](https://opencode.ai) website. Once installed, start a session in your project folder:

```bash
mkdir -p ~/scrapers/my-company-scraper
cd ~/scrapers/my-company-scraper
opencode
```

## Installing kilo.ai

Install kilo.ai according to the [kilo.ai](https://kilo.ai) documentation, then start a session:

```bash
cd ~/scrapers/my-company-scraper
kilo
```

## How to use them effectively

### Ask the AI to explain code

> "Explain what this Python script does line by line."

### Ask the AI to generate a scraper template

> "Create a Python scraper template that fetches jobs from a JSON API and pushes them to https://api.peviitor.ro/v1/scraper/jobs/upload/"

### Ask the AI to fix errors

> "My scraper got an HTTP 422 error when uploading jobs. Here is the error message: [...]. How do I fix it?"

### Ask the AI to review your code

> "Review this scraper for common peviitor push mistakes: missing company, unverified CIF, tracking URLs, unknown companies."

### Important: you are still responsible

AI can write code quickly, but **you must review and test everything**:

- Does the URL point to the real employer job page?
- Is the CIF/CUI verified against a trusted source?
- Did the job actually appear in peviitor search?

AI is a helper, not a replacement for your judgment. See [13 — Testing and verification](13-testing-verification.md) and [15 — Best practices](15-best-practices.md).

## Tips for volunteers

1. Start small: scrape one company, push 5 jobs, verify them.
2. Ask the AI to explain any error before trying to fix it.
3. Keep new scrapers in their own git repo (see [08](08-scraper-architecture.md)).
4. Commit often with clear messages (see [14](14-git-workflow.md)).
5. If the docs are unclear, open a [GitHub Issue](https://github.com/peviitor-ro/scraper-doc/issues/new/choose) and ask — see [19](19-questions-and-answers.md). Answers are added back to the docs.
