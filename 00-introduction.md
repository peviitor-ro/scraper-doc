# 00 — Introduction

Welcome to the [peviitor.ro](https://peviitor.ro) scraper documentation.

This repo is for new joiners and volunteers who want to build web scrapers that push job listings into the peviitor.ro search engine.

## What is peviitor.ro?

Peviitor is a Romanian jobs search engine. It aggregates open positions from company career pages and job boards so job seekers can find them in one place. The data schemas and validation rules live in [peviitor_core](https://github.com/peviitor-ro/peviitor_core#job-model-schema).

## What is a scraper?

A scraper is a small program that:

1. Fetches job listings from a company's career page or jobs API.
2. Validates and normalizes the data (title, company, CIF/CUI, location, URL, etc.).
3. Pushes the jobs to peviitor.ro via the [peviitor API](https://github.com/peviitor-ro/api).

## Where scrapers live

New scrapers are created in the [**peviitor-scrapers**](https://github.com/peviitor-scrapers) organization, not in personal accounts. The goal is to keep every active scraper organization-owned so anyone in the team can maintain it if a volunteer steps away.

For new work, we also recommend **one scraper per repository**. Every career page is built differently and scrapers break independently, so separate repos are easier to fix, test, and maintain. Some older repos live in personal accounts or group multiple scrapers; those are historical. See [08 — Scraper architecture](08-scraper-architecture.md) and [14 — Git workflow](14-git-workflow.md).

## Who this is for

This documentation is written for volunteers at [**Asociația Oportunități și Cariere (AOC)**](https://oportunitatisicariere.ro) who want to learn:

- How web scrapers work.
- How to build one from scratch.
- How to use AI tools such as [opencode.ai](https://opencode.ai) and [kilo.ai](https://kilo.ai) to write code faster.
- How to push data to a real search engine used by job seekers.

## What you will learn

By following this documentation you will:

- Understand the peviitor job and company models ([02](02-data-model.md), [04](04-company-model.md)).
- Know where to look up a company's CIF/CUI ([cuiscan.ro](https://cuiscan.ro), [cuifirma.ro](https://cuifirma.ro), and [09](09-source-of-truth.md)).
- Build a scraper that pushes jobs to [api.peviitor.ro](https://api.peviitor.ro).
- Verify that your pushed jobs actually appear in peviitor search ([13](13-testing-verification.md)).
- Manage your scraper in the peviitor-scrapers organization, with one repo per scraper ([14](14-git-workflow.md)).

## Questions?

Ask anything in a [GitHub Issue](https://github.com/peviitor-ro/scraper-doc/issues/new/choose); answers are added back to the relevant docs. See [19 — Questions and answers](19-questions-and-answers.md). For security or private matters, use [SECURITY.md](SECURITY.md).

Start with [01 — The story](01-the-story.md), or open the [full index](README.md).
