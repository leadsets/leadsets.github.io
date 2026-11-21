---
layout: default
title: "Leadsets MCP quickstart: connect Claude, Claude Code, Cursor and ChatGPT"
description: "Add the Leadsets remote MCP server (https://mcp.leadsets.ai) to Claude, Claude Code, Cursor, ChatGPT or any MCP client, then check the connection with get_usage."
---

# Leadsets MCP quickstart

Leadsets is a remote MCP server. Add one URL to your client and your agent gets eight B2B data tools: people and company search, enrichment, email verification, website and careers-page scraping, and a usage meter. Nothing to install and nothing to run locally.

- Server URL: `https://mcp.leadsets.ai`
- Auth: OAuth sign-in where the client supports it, otherwise an API key in an `Authorization: Bearer` header (keys start with `ls_live_`)
- Official setup page: [leadsets.ai/docs/mcp](https://www.leadsets.ai/docs/mcp)
- No account yet? [Get a free API key](https://www.leadsets.ai/signup). Free is 1,000 enrichments and 100 search records, no card.

## Claude (web and desktop)

1. Open **Customize**, then **Connectors**.
2. Click **+ Add**, then **Add custom connector**.
3. Name it `Leadsets` and paste `https://mcp.leadsets.ai`.
4. Click **Continue**, choose **Sign in now**, then **Add**, and sign in to Leadsets. It uses OAuth, so there is no key to paste.
5. In a chat, click **+**, then **Connectors**, and switch Leadsets on.

On Claude Team and Enterprise plans an owner adds the connector once under Organization settings, Connectors.

## Claude Code

```bash
claude mcp add --transport http leadsets https://mcp.leadsets.ai \
  --header "Authorization: Bearer $LEADSETS_KEY"
```

Start Claude Code and run `/mcp`. You should see `leadsets` with 8 tools.

## Cursor

Open Cursor Settings, then MCP, or edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "leadsets": {
      "url": "https://mcp.leadsets.ai",
      "headers": { "Authorization": "Bearer ls_live_..." }
    }
  }
}
```

Restart Cursor. You should see `leadsets` with 8 tools and a green dot.

## ChatGPT

ChatGPT adds MCP servers as plugins on the web. Custom servers are available on Pro (read-only) and on Business, Enterprise and Edu; on Business, an admin or owner creates it.

1. Open Plugins at `https://chatgpt.com/plugins`.
2. Click **+**, then **Add custom MCP server**.
3. Name it `Leadsets` and put `https://mcp.leadsets.ai` in Server URL.
4. Choose **OAuth** and sign in to Leadsets.
5. Confirm the warning, create it as a plugin, then type `@` in a new chat and pick Leadsets.

## Any other MCP client

Add a remote (HTTP) server with the URL `https://mcp.leadsets.ai`. If the client offers OAuth, sign in. Otherwise add the header `Authorization: Bearer ls_live_...`. Clients that read an `mcpServers` block take the same JSON as the Cursor example above.

## Check that it works

Ask your agent: **"What's my Leadsets usage?"** It should call `get_usage` and answer with what is left in your current windows. `get_usage` costs nothing, so it is a safe first call.

## Next steps

- [Tool reference](tools.html): what each of the eight tools takes and returns
- [Recipes](recipes.html): prompts and REST snippets for common GTM jobs
- [Limits and costs](https://www.leadsets.ai/docs/limits) and [pricing](https://www.leadsets.ai/pricing)

[Back to the Leadsets developer hub](./)
