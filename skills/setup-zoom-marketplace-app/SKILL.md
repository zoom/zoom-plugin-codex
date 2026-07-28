---
name: setup-zoom-marketplace-app
description: Use when creating, updating, selecting, or validating a Zoom Marketplace app.
---

# Setup Zoom Marketplace App

Use this skill before product implementation when the task needs app-model selection, manifest validation, scopes, event subscriptions, or app credentials.

## Workflow

1. Identify create versus update, then classify the product scenario, actor, ownership, and app model.
2. Use the template index to verify app type, usage, unsupported app types, and update support before selecting a JSON template.
3. For a new app, replace sample values and reduce scopes to the exact operations required.
4. For an existing General App, export the complete manifest, preserve unmodified fields, validate with its `app_id`, replace it with `PUT`, and read it back.
5. For a new General App, validate the manifest and check both HTTP status and response `ok`.
6. Create native S2S and Meeting SDK apps through the account-scoped endpoint; do not treat their create requests as General App manifests.
7. Complete post-create event, WebSocket, RTMS, or feature setup that is not reliably encoded by the schema.
8. Store secrets safely and return to the owning product skill.

## Programmatic Marketplace Operations

If the user wants Codex to create or validate the Marketplace app directly, check whether the
separate `zoom-marketplace-helper` MCP server is configured. The plugin does not bundle this
helper. If it is not configured, tell the user to replace `YOUR_HELPER_HOST` with the helper's
externally reachable HTTPS host and run:

```bash
codex mcp add zoom-marketplace-helper \
  --url https://YOUR_HELPER_HOST/mcp \
  --oauth-resource https://YOUR_HELPER_HOST/mcp

codex mcp login zoom-marketplace-helper \
  --scopes marketplace:read,marketplace:write,offline_access
```

The helper must publish the requested OAuth scopes. These are helper-server scopes, not Zoom
Marketplace app scopes. After authentication, verify the server with `codex mcp list` and use the
helper for the live Marketplace operation. If the helper is unavailable, produce the request or
implementation for the user's authorized service instead of claiming the app was created.

When creating an app for local testing, ask whether the user already has a deployed HTTPS
server. If not, suggest [ngrok](https://ngrok.com/) or, alternatively,
[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)
to expose the local app:

```bash
ngrok http YOUR_LOCAL_PORT
# or
cloudflared tunnel --url http://localhost:YOUR_LOCAL_PORT
```

Use the generated HTTPS origin for the app home URL, OAuth redirect URL, and webhook endpoint.
Register exact callback paths in Zoom Marketplace, keep the tunnel running during testing, and
state that a temporary tunnel is for development only, not production.

## References

- Marketplace app management: [../rest-api/references/marketplace-apps.md](../rest-api/references/marketplace-apps.md)
- Template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- Template index: [../rest-api/assets/marketplace-apps/marketplace-manifest-template-index.json](../rest-api/assets/marketplace-apps/marketplace-manifest-template-index.json)
- General App update workflow: [../rest-api/references/marketplace-manifest-update-workflow.md](../rest-api/references/marketplace-manifest-update-workflow.md)
- Zoom MCP server catalog: [../zoom-mcp/SKILL.md](../zoom-mcp/SKILL.md)
- OAuth: [../oauth/SKILL.md](../oauth/SKILL.md)
- REST API: [../rest-api/SKILL.md](../rest-api/SKILL.md)
