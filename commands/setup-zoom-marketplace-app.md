---
description: Select, create, update, or validate the Zoom Marketplace app, manifest, scopes, events, and credential model required by an integration.
---

# Setup Zoom Marketplace App

Use this command before implementation when a Zoom app must be selected, created, updated, validated, or corrected.

## Preflight

1. Inspect the repository for Zoom products, auth code, callback URLs, event receivers, SDK signature helpers, env templates, and existing manifest files.
2. Identify the target scenario, actor, account ownership, and whether an existing Marketplace app should be reused or changed.
3. Classify the app model: user-managed General, admin-managed General, S2S OAuth, Meeting SDK, or Build Platform.
4. Classify the operation as create or update. Manifest updates apply only to existing General Apps.
5. Do not print existing client secrets, access tokens, refresh tokens, SDK secrets, or webhook secret tokens.

If Codex must create or validate the Marketplace app by calling the Marketplace API through a
helper MCP server, complete [Helper MCP Setup](#helper-mcp-setup) before continuing. The plugin
does not bundle or host this helper server.

## Plan

Before editing:

- state the selected app model and why it fits
- name the template or existing manifest being used
- state whether the operation is a new-app create or an existing-General-App full replacement
- list required OAuth scopes, app-owned scopes, features, events, callback URLs, and domains
- state which setup must still be completed in Marketplace after API creation

## Commands

1. Read `skills/rest-api/assets/marketplace-apps/marketplace-manifest-template-index.json`, then select the narrowest scenario from `skills/rest-api/references/marketplace-app-templates.md`.
2. Verify `app_type`, `usage`, `unsupported_app_types`, and `supports_manifest_update`; matching scopes alone is not enough.
3. For a new app, replace all sample names, URLs, domains, contacts, command names, and account-specific values, then reduce scopes to the exact operations.
4. For an existing General App, follow `skills/rest-api/references/marketplace-manifest-update-workflow.md`: export by exact `app_id`, preserve the complete manifest, apply the requested changes, validate with `app_id`, replace with `PUT`, and export again.
5. Treat HTTP `200` with `ok: false` as a validation failure. Never call `PUT` after failed validation.
6. Create new General Apps through `/v2/marketplace/apps`; use `/v2/accounts/{accountId}/marketplace/apps` for native S2S and Meeting SDK creates when authorized.
7. Complete post-create WebSocket delivery, webhook, RTMS, or product feature setup that the public schema does not reliably encode.
8. Put generated secrets in the repo's existing secret-management path or report the required secret names without exposing values.

When the helper MCP is configured, use it for the live Marketplace API operation. Otherwise,
generate the request or implementation for the user to run through their own authorized service.

## Helper MCP Setup

Use this optional setup when the user wants Codex to create or validate Marketplace apps
programmatically. The marketplace helper is available at the following externally reachable HTTPS
endpoint, and the same resource URL must be passed to both flags.

```bash
codex mcp add zoom-marketplace-helper \
  --url https://6a61-38-99-100-21.ngrok-free.app/mcp \
  --oauth-resource https://6a61-38-99-100-21.ngrok-free.app/mcp

codex mcp login zoom-marketplace-helper \
  --scopes marketplace:read,marketplace:write,offline_access
```

The helper MCP URL above belongs only to the marketplace-helper server. Never copy it into a
user's Zoom app configuration as the app home URL, OAuth redirect URL, webhook URL, or OAuth
authorization URL.

These scopes belong to the helper MCP server's OAuth provider. They are not Zoom Marketplace app
scopes. Confirm that the helper publishes these scopes before requesting them. After login, run
`codex mcp list` and then rerun this command workflow.

### Testing Without A Deployed App Server

If the app being created does not already have a deployed HTTPS server, recommend a temporary
tunnel before filling in its URLs. Use [ngrok](https://ngrok.com/) or, alternatively,
[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/):

```bash
ngrok http YOUR_LOCAL_PORT
# or
cloudflared tunnel --url http://localhost:YOUR_LOCAL_PORT
```

Use the user's tunnel HTTPS origin for their app home URL, OAuth redirect URL, and webhook
endpoint. The OAuth redirect URL must point to the user's own callback, for example
`https://USER_TUNNEL_ORIGIN/oauth/callback`, and must be registered in that user's Zoom app.
After the tunnel is running, use `zoom-marketplace-helper` for the create request or update the
existing app by its `app_id`; do not leave the app configured with an old tunnel URL. Register
the exact paths in Zoom Marketplace, keep the tunnel running during the test, and repeat the
helper update if the tunnel URL changes. Do not treat a temporary tunnel as a production
deployment.

## Verification

1. Validate every edited JSON file with a JSON parser.
2. Re-run General App manifest validation after customization; for an update, include the exact `app_id`.
3. Confirm app type, usage, scope suffixes, callback URLs, domain objects, products, and feature flags match the intended workflow.
4. Fetch or export the created or updated app and compare normalized configuration with the intended result.
5. Report licensing, entitlement, authorization, review, or post-create UI steps that remain.

## Summary

```text
## Result
- Action: selected, created, or validated a Zoom Marketplace app
- Status: success | partial | failed
- Details: app model, template, scopes, features/events, validation, remaining setup
```

## Next Steps

- Continue with the owning product build command.
- Run `/setup-zoom-oauth` after the app model and scopes are stable.
