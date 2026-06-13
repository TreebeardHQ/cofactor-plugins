# @chatsdet/claude-plugin

Source of truth for the **canary-mcp** Claude Code plugin and the Claude.ai
custom-connector branding. Published into the public marketplace repo
[`TreebeardHQ/cofactor-plugins`](https://github.com/TreebeardHQ/cofactor-plugins)
via `scripts/sync-claude-plugin.sh`.

Contents:

- `.claude-plugin/plugin.json` — plugin manifest; declares the hosted HTTP
  MCP server at `https://mcp.trycanary.ai/mcp`.
- `connector/manifest.json` — metadata used when submitting Canary to
  Claude.ai (Settings → Connectors).
- `connector/icon-512.png` — connector branding, served by the MCP server at
  `https://mcp.trycanary.ai/icon.png`.

This package is private to the workspace. Distribution to end users happens
through the public marketplace repo, never npm.

## Install in Claude Code

```text
/plugin marketplace add TreebeardHQ/cofactor-plugins
/plugin install canary-mcp@cofactor-plugins
```

Claude Code discovers
`https://mcp.trycanary.ai/.well-known/oauth-protected-resource` and walks
the user through OAuth 2.1 + PKCE in the browser. The hosted MCP server
uses Dynamic Client Registration, so no client secret is needed.

## Install in Claude.ai

Open [https://trycanary.ai/connect](https://trycanary.ai/connect) and click
**Connect to Claude**, or paste the MCP URL `https://mcp.trycanary.ai/mcp`
into Settings → Connectors → Add custom connector.

## Other MCP clients

The server is a standards-compliant Streamable HTTP MCP endpoint. Cursor,
Windsurf, and any other MCP-aware client can use the same URL with OAuth
2.1.

## Publishing a new version

1. Bump `version` in `.claude-plugin/plugin.json`.
2. Land the change on `main` in this repo.
3. Run `scripts/sync-claude-plugin.sh --apply` from the monorepo root. It
   mirrors this directory into `plugins/canary-mcp/` in the public
   marketplace repo and regenerates the top-level `marketplace.json`.
   Claude Code clients pick up the new version on next
   `/plugin marketplace update cofactor-plugins`.

## Public docs

The `/integrations/claude` docs page is managed via the canary CLI (docs
are database-backed, not MDX in this repo). Create or update it with:

```bash
canary docs upsert --slug integrations/claude --env prod
```

`connector/manifest.json` → `documentation` already points at
`https://docs.trycanary.ai/integrations/claude`.
