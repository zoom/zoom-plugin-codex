---
name: zoom-mcp
description: Use when selecting or using an official Zoom-hosted MCP server, checking its endpoint, routing to the right product surface, or choosing the matching Marketplace app template and OAuth scopes.
---

# Zoom MCP Servers

Use this skill when a task needs an official Zoom-hosted MCP server. Select the narrowest
product-specific server, use its canonical endpoint, and then discover the live tools with
`tools/list` before invoking anything. The hosted server catalog can change independently of
this plugin.

## Server Catalog

The current official catalog contains seven server surfaces. All canonical endpoints use
`mcp.zoom.us`.

| Server | Canonical streamable HTTP endpoint | Current tool count | Primary use |
|---|---|---:|---|
| Zoom MCP | `https://mcp.zoom.us/mcp/zoom/streamable` | 9 | Cross-Zoom search, meeting assets, recordings, Docs, and Hub content |
| Meetings MCP | `https://mcp.zoom.us/mcp/meeting/streamable` | 4 | Meeting search, meeting assets, recording lists, and recording resources |
| Docs MCP | `https://mcp.zoom.us/mcp/docs/streamable` | 2 | Create Docs from Markdown and retrieve file content |
| Tasks MCP | `https://mcp.zoom.us/mcp/tasks/streamable` | 20 | Tasks, comments, assignees, collaborators, and task steps |
| Revenue Accelerator MCP | `https://mcp.zoom.us/mcp/revenue_accelerator/streamable` | 15 | Conversation, deal, CRM, scorecard, indicator, team, and user intelligence |
| Chat MCP | `https://mcp.zoom.us/mcp/chat/streamable` | 20 | Chat messages, channels, contacts, files, sessions, search, and writes |
| Whiteboard MCP | `https://mcp.zoom.us/mcp/whiteboard/streamable` | 11 | Whiteboard creation, retrieval, listing, and collaboration |

Chat also exposes the legacy alias
`https://mcp.zoom.us/mcp/team_chat/streamable`; use `/mcp/chat/streamable` for new
configurations.

The detailed catalog, matching templates, and scope families are in
[references/servers.md](references/servers.md). The live MCP `tools/list` response is the
authority for the exact current tool schema and availability.

## Authentication And Routing

1. Choose the server and matching user-managed General App template from
   [Marketplace app templates](../rest-api/references/marketplace-app-templates.md).
2. Request only the scopes needed by the selected server's tools. Do not combine unrelated
   server scope sets by default.
3. Complete Zoom user OAuth with PKCE when the client cannot safely hold a secret. Zoom's
   hosted MCP servers require manual client registration; do not assume Dynamic Client
   Registration is available.
4. Pass the resulting bearer token to the selected MCP endpoint and run `tools/list`.
5. Route deterministic CRUD or bulk automation to the corresponding Zoom REST API skill when
   the MCP surface does not expose the required operation.

The plugin documents these hosted servers and their Marketplace templates. It does not bundle
the remote Zoom MCP services or create a local `.mcp.json` registration for them.

## References

- [Server catalog, endpoints, tools, and scope families](references/servers.md)
- [Marketplace template selector](../rest-api/references/marketplace-app-templates.md)
- [OAuth guidance](../oauth/SKILL.md)
- [REST API fallback](../rest-api/SKILL.md)
- Official connection guide: https://developers.zoom.us/docs/mcp/servers/connect-to-zoom-mcp-servers/
