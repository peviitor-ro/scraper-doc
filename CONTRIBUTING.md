# Contributing

Thanks for improving the peviitor scraper documentation.

## Making a change

1. Create a branch from `main`.
2. Edit the relevant numbered file. The full list is in [README.md](README.md).
3. Check that relative links still resolve and that examples match the current schemas.
4. Commit with a clear message, for example: `Fix CIF rules in source-of-truth doc`.
5. Open a pull request and describe what changed and why.

## Documentation rules

- Treat [peviitor_core](https://github.com/peviitor-ro/peviitor_core#job-model-schema) as the source of truth for job and company fields.
- Treat the [API repository](https://github.com/peviitor-ro/api) as the source of truth for endpoints, methods, and payloads.
- Use generic paths such as `~/scrapers/...`; never commit personal home-directory paths.
- Use clearly fictional company names and CUI values in examples, or verify real ones before publishing.
- Never commit API keys, tokens, passwords, CVs, candidate data, or other personal data.
- Keep GDPR guidance aligned with [16 — GDPR and data minimization](16-gdpr-data-minimization.md).

## Quick links

- [Documentation index](README.md)
- [Data model](02-data-model.md)
- [Company model](04-company-model.md)
- [Best practices](15-best-practices.md)
