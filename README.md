# Buzz Claude Code Plugin

Connect Claude Code to Buzz — Eon's internal AI assistant — via this plugin. Bundles the Buzz MCP server into a single installable package, so Claude can ask Buzz questions that need Eon-internal data or actions.

## Capabilities

- **`ask_buzz` tool** — forwards a question to Buzz's multi-agent orchestrator, which routes it to the right in-house specialist and returns a single synthesized answer. Buzz has live, authenticated access to Eon's internal systems:
  - **Revenue & sales** — Salesforce (ARR, pipeline, deals, forecasts), Gong, HubSpot, LinkedIn, Nooks.
  - **Analytics & SQL** — Snowflake and the production database (backup/storage/inventory statistics, ad-hoc queries).
  - **Infra triage** — Temporal workflows, Groundcover logs/metrics/traces, GitHub, Linear, codebase root-cause analysis.
  - **Support & customers** — Zendesk, Admin Service.
  - **Knowledge & comms** — Slack, Gmail, Google Drive, Notion, product docs.

Answers are scoped to the authenticated user's connected accounts and permissions.

## Installation

### Claude Code

Run inside Claude Code:

```
/plugin marketplace add eon-io/claude-code-buzz-plugin
/plugin install buzz@buzz
```

### claude.ai / Claude Desktop (custom connector)

No plugin needed — add a custom connector pointing at:

```
https://mcp.buzz.eon.io/mcp
```

Sign in with Okta when prompted — the server handles the OAuth flow and already allowlists the claude.ai callback.

## MCP Server

The plugin connects to `https://mcp.buzz.eon.io/mcp` via HTTP. Authentication is Okta OIDC, handled by the server (FastMCP OIDC proxy) — you'll be prompted to sign in on first use.

The server source lives in the [buzz repo](https://github.com/eon-io/buzz) under `buzz/buzz_mcp/`; it is a thin proxy that forwards prompts to Buzz's `POST /api/agent/run` endpoint.

## Tips

- Ask **one complete, self-contained question** — Buzz doesn't see your Claude conversation, so include the entities and timeframe that matter.
- Data-heavy questions can take from ~30 seconds to a few minutes; that's normal.

## Local Development

```bash
claude --plugin-dir .
```

Run `/reload-plugins` inside Claude Code after making local edits.
