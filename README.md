# scraper-doc

Documentation for volunteers building [peviitor.ro](https://peviitor.ro) company job scrapers.

Peviitor aggregates open positions from company career pages so job seekers can find them in one place. This repo explains the job and company data models, how to look up a company's CIF/CUI, how to upload jobs to the [peviitor API](https://github.com/peviitor-ro/api), how to verify the result, and how to keep scrapers GDPR-friendly.

New scrapers belong in the [peviitor-scrapers](https://github.com/peviitor-scrapers) organization so the team can maintain them even if a volunteer steps away.

## Documentation index

| # | Topic |
|---|-------|
| 00 | [Introduction](00-introduction.md) |
| 01 | [The story](01-the-story.md) |
| 02 | [Data model](02-data-model.md) |
| 03 | [Push and verify](03-push-and-verify.md) |
| 04 | [Company model](04-company-model.md) |
| 05 | [Geographic scope](05-geographic-scope.md) |
| 06 | [Reference scrapers](06-reference-scrapers.md) |
| 07 | [Installing AI tools](07-installing-ai-tools.md) |
| 08 | [Scraper architecture](08-scraper-architecture.md) |
| 09 | [Source of truth: CIF/CUI and company name](09-source-of-truth.md) |
| 10 | [Creating a company in peviitor core](10-company-creation.md) |
| 11 | [Building a scraper step by step](11-building-a-scraper-step-by-step.md) |
| 12 | [Scraper template](12-scraper-template.md) |
| 13 | [Testing and verification](13-testing-verification.md) |
| 14 | [Git workflow](14-git-workflow.md) |
| 15 | [Best practices and common pitfalls](15-best-practices.md) |
| 16 | [GDPR and data minimization](16-gdpr-data-minimization.md) |
| 17 | [Scraper examples](17-scraper-examples.md) |
| 18 | [Components of a scraper repository](18-scraper-components.md) |
| 19 | [Questions and answers](19-questions-and-answers.md) |

If you are in a hurry, read [00 — Introduction](00-introduction.md), [02 — Data model](02-data-model.md), [09 — Source of truth](09-source-of-truth.md), [13 — Testing and verification](13-testing-verification.md), and [16 — GDPR](16-gdpr-data-minimization.md).

## Contributing

Corrections and improvements are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

New team member? Open an [onboarding issue](https://github.com/peviitor-ro/scraper-doc/issues/new?template=onboarding.md) with your GitHub user and Discord ID.

Repo policies: [Code of Conduct](CODE_OF_CONDUCT.md) · [Security Policy](SECURITY.md) · [Changelog](CHANGELOG.md)

## Questions?

Ask anything in a [GitHub Issue](https://github.com/peviitor-ro/scraper-doc/issues/new/choose). We encourage as many questions as possible; answers are added back to the docs. See [19 — Questions and answers](19-questions-and-answers.md) for the full process.

For security issues or anything involving private data, follow [SECURITY.md](SECURITY.md) instead of opening a public issue.

## Maintainers and contact

- Website: [oportunitatisicariere.ro](https://oportunitatisicariere.ro)
- [@lavilapustea24](https://github.com/lavilapustea24) — organization coordinator
- [@sebiboga](https://github.com/sebiboga) — repository maintainer

For questions or corrections, open an issue in this repository.

## Sources of truth

- [peviitor_core](https://github.com/peviitor-ro/peviitor_core#job-model-schema) — job and company schemas
- [api.peviitor.ro source](https://github.com/peviitor-ro/api) — API implementation and OpenAPI spec
- [peviitor-scrapers](https://github.com/peviitor-scrapers) — canonical organization for scraper repositories

## License

MIT — see [LICENSE](LICENSE).
