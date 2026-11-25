---
layout: default
title: "Leadsets recipes: prompts and code for prospect lists, verification and hiring signals"
description: "Copy-paste recipes for the Leadsets MCP server and REST API: build an ICP list, verify a list before sending, find the best contact at an account, watch hiring signals and pace a large run."
---

# Leadsets recipes

Short, tested patterns for the Leadsets MCP server and REST API. The prompts work in Claude, Claude Code, Cursor, ChatGPT or any MCP client once Leadsets is connected ([MCP quickstart](mcp-quickstart.html)). The code uses the official SDKs.

## 1. Check what is left before a big run

Prompt:

> Check my Leadsets usage and tell me how many search and enrichment records I have left in this five-hour window and this week.

`get_usage` costs nothing. Agents should call it before any large job and split the job across windows if it would cross a limit.

## 2. Build an ICP list with match reasons

Prompt:

> Using Leadsets, find 25 VP or Head of Sales contacts at US SaaS companies with 50 to 200 employees, only people with a verified work email. Return a table with name, title, company, match reasons, email, verification status and checked_at.

Python:

```python
import os
from leadsets import Leadsets

ls = Leadsets(os.environ["LEADSETS_KEY"])
res = ls.people.search(
    seniority=["vp", "head"],
    function=["revenue"],
    employees="50-200",
    country=["US"],
    industry=["saas"],
    has_verified_email=True,
    limit=25,
)
for p in res.items:
    print(p.full_name, p.title, p.match_reasons)
print(res.usage)
```

Pass `next_cursor` back as `cursor` for the next page. A search that finds nothing counts nothing.

## 3. Size a market before you pull it

Set `count_only` on a people search to get only the number of matches (counting stops at 10,000). It uses no records, so an agent can test several filter combinations first and then pull the best one.

## 4. Verify a list before it goes to your sequencer

Prompt:

> Verify these emails with Leadsets. Put verified addresses in one table, catch_all and unknown in a second table labelled hold, and drop invalid ones: ...

TypeScript:

```ts
import { Leadsets } from "@leadsetsai/sdk";

const ls = new Leadsets(process.env.LEADSETS_KEY);
const res = await ls.emails.verify({
  emails: ["maya@northwind.io", "sales@stackfold.io", "old.address@parcelry.co"],
});
console.log(res.summary); // counts of verified, catch_all, unknown, invalid, not_checked
```

Up to 100 emails per call. Each email that gets a status uses one enrichment record.

## 5. Best contact at one target account

Use `priorities` on `search_people` with one `company_domain` and up to ten groups of titles, best first, for example founders and CEOs first, then heads of growth and VPs of sales. Results fill from the first group, never repeat a person and put verified emails first. Add `include_emails: true` to get their work emails (1 enrichment record each).

## 6. Hiring signals from careers pages

Prompt:

> Use Leadsets scrape_jobs on these five company domains and list any open sales or marketing roles with title, team, location and date.

Scraping uses scrape units, an add-on on paid plans (1 unit is one standard page). For a ready-made watcher that prints only new roles, see [job-intent-agent](https://github.com/leadsets/job-intent-agent).

## 7. Account research in one pass

Prompt:

> For acme.com, run Leadsets enrich_company and scrape_company, then summarise what the company sells, who its named customers are, its headcount band and the three links a seller should read first.

## More

- [gtm-recipes](https://github.com/leadsets/gtm-recipes): ICP list builders, verification scripts and MCP prompts
- [n8n-workflows](https://github.com/leadsets/n8n-workflows): ready-to-import n8n workflows
- [leadsets-skills](https://github.com/leadsets/leadsets-skills): fifty free, MIT-licensed SKILL.md playbooks
- [Leadsets docs](https://www.leadsets.ai/docs) · [Pricing](https://www.leadsets.ai/pricing) · [Join the waitlist](https://www.leadsets.ai/signup) (public launch 10 December 2026)

[Back to the Leadsets developer hub](./)
