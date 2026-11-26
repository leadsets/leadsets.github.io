---
layout: default
title: "Leadsets tool reference: the eight MCP and REST tools"
description: "What each Leadsets tool does, what it costs against your plan meters, and the REST route behind it: search_people, search_companies, enrich_person, enrich_company, verify_email, scrape_company, scrape_jobs, get_usage."
---

# Leadsets tool reference

Every Leadsets tool is available both as an MCP tool on `https://mcp.leadsets.ai` and as a REST route on `https://api.leadsets.ai/v1`. Full parameter docs live at [leadsets.ai/docs](https://www.leadsets.ai/docs).

| MCP tool | REST route | What it does | What it uses |
|---|---|---|---|
| `search_people` | `POST /v1/people/search` | Find people by title, function, seniority, location, company size and industry. Every result says why it matched. | 1 search record per record returned (person, company), 1 enrichment record per email, 1 request per call |
| `search_companies` | `POST /v1/companies/search` | Find accounts by what they do, industry, headcount band, technology and location. | 1 search record per record returned |
| `enrich_person` | `POST /v1/people/enrich` | Current role, company, and a verified work email when one exists. | 1 enrichment record per record returned |
| `enrich_company` | `POST /v1/companies/enrich` | Firmographics for a domain, with source and freshness on every field. | 1 enrichment record per record returned |
| `verify_email` | `POST /v1/emails/verify` | One email or up to 100 per call, each labelled verified, catch_all, unknown or invalid, with a confidence score. | 1 enrichment record per email that gets a status |
| `scrape_company` | `POST /v1/scrape/company` | A company homepage as structured JSON: products, positioning, named customers and key links. | 1 scrape unit per page read (add-on) |
| `scrape_jobs` | `POST /v1/scrape/jobs` | Open roles from the company's careers page, job board and LinkedIn, with titles and dates, for hiring signals. | 1 scrape unit per role returned (add-on) |
| `get_usage` | `GET /v1/usage` | What is used and what is left on every meter, with reset times. | Nothing |

In people search, each person is one record and their company another: pass `include_company: false` for people only. Work emails come with `include_emails: true` (or `has_verified_email`) and use 1 enrichment record each.

An empty result uses 0 records and 1 request. A `count_only` people search returns only how many people match and uses no records.

## search_people filters

From the [people search reference](https://www.leadsets.ai/docs/api/people-search):

- `title` (fuzzy-matched, so "VP Sales" also finds "Vice President, Sales")
- `seniority`: `c_level`, `vp`, `head`, `director`, `manager`, `ic`
- `function`: `revenue`, `marketing`, `engineering`, `product`, `ops`, `finance`, `hr`
- `company_domain`, `employees` (a range such as `"50-200"`), `industry` (slugs such as `"saas"`), `country` (ISO 3166-1 alpha-2)
- `has_verified_email` to return only people with a verified work email
- `include_company` (default true) and `include_emails` (default false, on with `has_verified_email`)
- `company_keywords` and `exclude_company_keywords` to match what the company does
- `max_age_days` and `exclude_stale` for freshness
- `priorities` to rank the best contacts at one company by groups of titles
- `limit` (1 to 100, default 25; at most 10 per call on the Free plan) and `cursor` for paging

Each record carries `match_reasons`, and each email carries a `status` and a `checked_at` date.

## verify_email statuses

| Status | Meaning | What an agent should do |
|---|---|---|
| `verified` | The mailbox exists and accepts mail. | Safe to send. |
| `catch_all` | The domain accepts every address, so this mailbox cannot be confirmed. | Send with care, or hold for review. |
| `unknown` | The check could not decide (for example a timeout or a full inbox). | Hold, and re-check later. |
| `invalid` | The address does not exist or cannot receive mail. | Do not send. |

Uncertain results are not rounded up to valid, and invalid emails are not served as contacts.

## Limits and errors

Paid plans are a flat monthly fee (Solo $29, Pro $99, Team $399, Max $999) with published fair-use limits per rolling five hours and per week, plus a concurrent-search cap. Over a limit, the API answers `429` with a `Retry-After` header and a machine-readable `error.type` (`window_full`, `weekly_limit` or `too_many_concurrent`). Paid calls accept an `Idempotency-Key`, so retries never count twice; the SDKs send one for you. See [limits and costs](https://www.leadsets.ai/docs/limits).

## SDKs

```bash
npm i @leadsetsai/sdk
pip install leadsets
```

Source: [leadsets/sdk](https://github.com/leadsets/sdk) (TypeScript) and [leadsets/sdk-python](https://github.com/leadsets/sdk-python).

[Back to the Leadsets developer hub](./) · [MCP quickstart](mcp-quickstart.html) · [Recipes](recipes.html)
