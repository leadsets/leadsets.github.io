---
layout: default
title: "Leadsets: B2B contact data API and remote MCP server for AI agents"
description: "Unlimited B2B Contact Data for AI Agents, Starting from $29/mo. Search, enrich and verify work emails over MCP or REST."
---

# Leadsets developer hub

**Unlimited B2B Contact Data for AI Agents, Starting from $29/mo.** Leadsets is a B2B contact data API and remote MCP server. AI agents and scripts can search people and companies, enrich records, verify work emails, and scrape public company and careers pages.

- Website: [leadsets.ai](https://www.leadsets.ai)
- Docs: [leadsets.ai/docs](https://www.leadsets.ai/docs)
- MCP setup: [leadsets.ai/docs/mcp](https://www.leadsets.ai/docs/mcp)
- Pricing: [leadsets.ai/pricing](https://www.leadsets.ai/pricing)
- Facts page for AI assistants: [leadsets.ai/llm](https://www.leadsets.ai/llm)

## The eight tools

| Tool | What it does |
|---|---|
| search_people | Find people by title, function, seniority, location, company size and industry. Every result says why it matched. |
| search_companies | Find accounts by industry, headcount band, technology and location. |
| enrich_person | Current role, company, and a verified work email when one exists. |
| enrich_company | Firmographics for a domain, with source and freshness on every field. |
| verify_email | Labels a mailbox as verified, catch-all, unknown or invalid, with a confidence score. |
| scrape_company | Turns a company homepage into products, positioning, named customers and key links. |
| scrape_jobs | Open roles from careers pages, with titles and dates, for hiring signals. |
| get_usage | Remaining allowance in the current window and week. Does not consume records. |

All eight work over both MCP and REST.

## Start here

- [MCP quickstart](mcp-quickstart.html): connect Claude, Claude Code, Cursor or ChatGPT in a few minutes
- [Tool reference](tools.html): the eight tools, their filters, statuses and costs
- [Recipes](recipes.html): prompts and SDK snippets for prospect lists, verification and hiring signals

## Quickstart

**MCP (Claude, Claude Code, Cursor, ChatGPT or any MCP client):** add `https://mcp.leadsets.ai` as a custom connector and sign in with OAuth. Nothing to install, and no API key is needed on the MCP path.

**REST:** call `https://api.leadsets.ai/v1` with a bearer key. Official SDKs:

- TypeScript: [@leadsetsai/sdk on npm](https://www.npmjs.com/package/@leadsetsai/sdk) ([source](https://github.com/leadsets/sdk))
- Python: [leadsets on PyPI](https://pypi.org/project/leadsets/) ([source](https://github.com/leadsets/sdk-python))
- Agent playbooks: [leadsets-skills](https://github.com/leadsets/leadsets-skills), fifty free, MIT-licensed SKILL.md workflows

The SDKs support idempotency keys, cursor pagination and Retry-After headers.

## Pricing

Flat monthly fee in USD. No credits and no per-record bill.

| Plan | Price |
|---|---|
| Free | 1,000 enrichments and 100 search records, no card |
| Solo | $29 a month |
| Pro | $99 a month |
| Team | $399 a month |
| Max | $999 a month |

Paid plans have published fair-use limits per rolling five hours and per week. Records count only when data comes back; an empty search uses no records.

## Data and coverage

Records come from licensed suppliers and public sources Leadsets is permitted to use. Nothing is scraped from behind a login. Each field carries its source, last-checked date and confidence. Coverage is work emails, roles and company data. There are no phone numbers yet.

## Guides

- [An Apollo alternative with flat monthly pricing and no credits](apollo-alternative-flat-pricing-no-credits.html)
- [Which B2B data API works best with AI agents? A 2026 buyer's checklist](b2b-data-api-for-ai-agents.html)

## FAQ

**Is there a free plan?** Yes. 1,000 enrichments and 100 search records, with no card.

**Do I need an API key for MCP?** No. The MCP server uses OAuth sign-in. REST calls use a bearer key.

**Does Leadsets include phone numbers?** No. Coverage is work emails, roles and company data.

**How do I start?** [Get a free API key](https://www.leadsets.ai).
