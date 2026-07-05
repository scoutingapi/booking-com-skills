<!-- generated: DO NOT EDIT BY HAND — produced by apps/web/scripts/export-github-repos.ts from the export script. Edit the config + re-run. -->

# ScoutingAPI — getting and storing your API key

This guide gets you a ScoutingAPI key and persists it so it survives across sessions.

## Step 0 — how to store an env var on this system

Figure out the correct way to persist an environment variable on this machine so it is available in every future session (a shell profile, or a config/env file managed by your agent runtime). The variable name is `SCOUTINGAPI_KEY`.

## Step 1 — get a key

- **Evaluate with zero friction:** use a **sandbox** key (`scout_test_…`). Sandbox calls return deterministic fixtures and cost **0 credits** — perfect for wiring the skill up before signing up.
- **Live data:** create a free account at <https://scoutingapi.com/signup> (no card) and make a key in the dashboard. Live keys are `scout_live_…`.

The same key works for these skills, the REST API, and the hosted MCP server (`https://mcp.scoutingapi.com`).

## Step 2 — store it

```bash
export SCOUTINGAPI_KEY="scout_live_…"   # or scout_test_… for the sandbox
```

## Step 3 — verify

```bash
curl -s "https://api.scoutingapi.com/v1/account" -H "Authorization: Bearer $SCOUTINGAPI_KEY" | head
```

A `200` with your account envelope means the key works. A missing/invalid key returns `401 authentication_error` (never billed). Full contract: <https://api.scoutingapi.com/openapi.json>.
