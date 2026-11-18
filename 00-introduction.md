# 00 — Introduction

Welcome to the peviitor.ro scraper documentation.

This folder is for new joiners and volunteers who want to learn how to build web scrapers that push job listings into the peviitor.ro search engine.

## What is peviitor.ro?

Peviitor is a Romanian jobs search engine. It aggregates open positions from company career pages and job boards so job seekers can find them in one place.

## What is a scraper?

A scraper is a small program that:
1. Fetches job listings from a company's career page or jobs API.
2. Validates and normalizes the data (title, company, CIF/CUI, location, URL, etc.).
3. Pushes the jobs to peviitor.ro via its API.

## The philosophy: 1 scraper = 1 repo

Romania has over 1 million companies. Each company needs its own scraper because:
- Every career page is built differently.
- Every jobs API returns data in a different format.
- Scrapers break independently and need to be fixed independently.

So we follow the rule: **one scraper, one repository**.

## Who this is for

This documentation is written for volunteers at **Asociatia Oportunitati si Carriere** who want to learn:
- How web scrapers work.
- How to build one from scratch.
- How to use AI tools (opencode.ai, kilo.ai) to write code faster.
- How to push data to a real search engine used by job seekers.

## What you will learn

By following this documentation you will:
- Understand the peviitor job and company data models.
- Know where to look up a company's CIF/CUI (cuiscan.ro, cuifirma.ro).
- Build a scraper that pushes jobs to `api.peviitor.ro`.
- Verify that your pushed jobs actually appear in peviitor search.
- Manage your scraper in its own git repository.

Let's get started.
