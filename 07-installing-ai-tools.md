# 07 — Installing and Using AI Tools

You will use **opencode.ai** and **kilo.ai** to help write your scraper. These are AI coding assistants that can generate code, explain errors, and suggest fixes.

## Installing opencode.ai

opencode.ai is typically installed as a CLI tool or desktop application. Follow the installation instructions on the opencode.ai website.

Once installed, you can start a session in your project folder:

```bash
opencode
```

Or point it at your scraper repository:

```bash
cd /path/to/your-scraper-repo
opencode
```

## Installing kilo.ai

kilo.ai is another AI assistant focused on software engineering tasks. Install it per the kilo.ai documentation, then start a session:

```bash
kilo
```

Or in your project folder:

```bash
cd /path/to/your-scraper-repo
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
> "Review this scraper for common peviitor push mistakes: missing CIF, ampersands in URLs, unknown companies."

### Important: You are still responsible

AI can write code quickly, but **you must review and test everything**:
- Does the URL look correct?
- Is the CIF/CUI valid?
- Did the job actually appear in peviitor search?

AI is a helper, not a replacement for your judgment.

## Tips for volunteers

1. Start small: scrape one company, push 5 jobs, verify them.
2. Ask the AI to explain any error before trying to fix it.
3. Keep your scraper in its own git repo (one scraper = one repo).
4. Commit often with clear messages.
