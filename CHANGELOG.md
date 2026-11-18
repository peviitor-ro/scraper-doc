# Changelog

All notable changes to this documentation repository are recorded here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- `19-questions-and-answers.md`, a GitHub question issue template, and guidance for asking questions; answers will be folded back into the docs.
- GitHub onboarding issue template for new team members (GitHub user and Discord ID only, no full names).

### Changed

- Explicitly excluded full names from the onboarding and question issue templates for GDPR reasons.
- Clarified that new and active scrapers belong in the `peviitor-scrapers` organization, with one repo per scraper, so maintainers can take over when volunteers leave.
- Made the architecture in `08` language-agnostic: the same structure applies to TypeScript, PHP, JavaScript, C, Rust, and other languages.

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
