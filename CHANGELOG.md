# Changelog

All notable changes to this documentation repository are recorded here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

## [2026-11-18]

### Added

- Initial scraper documentation (`00`–`16`) for volunteers.
- `README.md` with the full documentation index.
- `LICENSE` (MIT), `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, and `SECURITY.md`.
- `17-scraper-examples.md` with 50 Python and Node.js scraper links.
- `18-scraper-components.md`, based on the peviitor-scrapers organization README and the EPAM reference scraper.
- Maintainers/contact section, repository homepage, and GitHub topics.

### Changed

- Rewrote the early notes and added headings, links, and consistent formatting.
- Corrected schema/API guidance: optional `location`, `expirationdate`, and CIF/CUI rules.
- Standardized local paths to `~/scrapers/...` and the audit trail to `data/uploaded_urls.txt`.
- Updated the Python template to write `data/uploaded_urls.txt` and support `DRY_RUN`.
- Softened the “one scraper = one repo” rule to a convention for new work.
