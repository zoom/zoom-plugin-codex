# Zoom

Zoom connects Codex to Zoom meeting context through the Zoom app connector and provides developer workflows for planning, building, debugging, and reviewing Zoom integrations across APIs, SDKs, webhooks, WebSockets, bots, and automation use cases.

## Plugin Shape

This repository is packaged as a Codex plugin:

- plugin manifest: [`.codex-plugin/plugin.json`](.codex-plugin/plugin.json)
- Zoom app mapping: [`.app.json`](.app.json)
- local marketplace metadata for sideload testing: [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json)
- deterministic command workflows: [`commands/`](commands/)
- focused reviewer agents: [`agents/`](agents/)
- reusable workflows and references: [`skills/`](skills/)
- branding and screenshot assets: [`assets/`](assets/)
- local sideload guide: [`sideload.md`](sideload.md)

This plugin contains the Zoom app connector mapping, local developer guidance, commands, skills, reviewer agents, and branding assets.

## What Zoom Does In Codex

Use `Zoom` when you want Codex to:

- search Zoom meetings by topic, attendee, or content
- retrieve summaries, transcripts, recordings, and related meeting assets
- pull meeting context into coding, documentation, or follow-up workflows
- route Zoom MCP requests to the correct hosted product server
- choose the right Zoom product surface for an integration
- build Zoom REST API, SDK, webhook, WebSocket, bot, and automation workflows
- debug Zoom auth, event delivery, SDK, and API issues

## Zoom MCP Servers

The Zoom app connector can use the official Zoom-hosted MCP surfaces below. All canonical
endpoints use `mcp.zoom.us`; the detailed tool and scope matrix is in the
[`zoom-mcp` skill](skills/zoom-mcp/SKILL.md).

| Server | Endpoint | Use |
|---|---|---|
| Zoom MCP | `https://mcp.zoom.us/mcp/zoom/streamable` | Cross-Zoom search, meeting assets, recordings, Docs, and Hub content |
| Meetings MCP | `https://mcp.zoom.us/mcp/meeting/streamable` | Meeting search, assets, and recordings |
| Docs MCP | `https://mcp.zoom.us/mcp/docs/streamable` | Create Docs from Markdown and retrieve file content |
| Tasks MCP | `https://mcp.zoom.us/mcp/tasks/streamable` | Tasks, comments, assignees, collaborators, and task steps |
| Revenue Accelerator MCP | `https://mcp.zoom.us/mcp/revenue_accelerator/streamable` | Conversation, deal, CRM, scorecard, and indicator intelligence |
| Chat MCP | `https://mcp.zoom.us/mcp/chat/streamable` | Chat messages, channels, contacts, files, sessions, search, and writes |
| Whiteboard MCP | `https://mcp.zoom.us/mcp/whiteboard/streamable` | Whiteboard creation, retrieval, and collaboration |

The legacy Chat endpoint `https://mcp.zoom.us/mcp/team_chat/streamable` remains available;
use `/mcp/chat/streamable` for new configurations. Use the matching Marketplace template and
request only the scopes required by the selected server's tools.

## Local Testing

For local sideload testing, see [`sideload.md`](sideload.md).

## Using In Codex

Codex can use this plugin through the Zoom app connector plus command, skill, and reviewer-agent surfaces:

- install the plugin from `/plugins`
- authenticate the Zoom app connector when Codex prompts for it
- mention the plugin as `@Zoom` if the UI exposes it
- use natural language requests that need live Zoom meeting context
- use slash commands for deterministic flows such as `/setup-zoom-oauth`, `/debug-zoom-auth`, `/debug-zoom-webhook`, and `/zoom-integration-doctor`
- invoke a bundled skill explicitly with `$skill-name`, for example `$start` or `$setup-zoom-oauth`
- describe the task naturally and let Codex route into the right skill

The Zoom app connector auth is managed by Codex. It is not exposed as a shell environment variable or raw bearer token.

If a newly installed skill does not trigger in the current thread, start a new Codex session so the plugin metadata reloads.

## Command Workflows

Use the bundled slash commands when you want a deterministic flow rather than open-ended routing:

| Command | Description |
|---|---|
| [`/plan-zoom-product`](commands/plan-zoom-product.md) | Choose the right Zoom product surface for a use case and explain the tradeoffs clearly |
| [`/plan-zoom-integration`](commands/plan-zoom-integration.md) | Turn a Zoom product idea into a practical build plan with auth, architecture, and milestones |
| [`/setup-zoom-marketplace-app`](commands/setup-zoom-marketplace-app.md) | Select, create, update, or validate the app model, manifest, scopes, events, and credentials for a Zoom integration; optionally use a helper MCP server for live API operations |
| [`/debug-zoom`](commands/debug-zoom.md) | Triage a broken Zoom integration when the failing layer is not yet obvious |
| [`/setup-zoom-oauth`](commands/setup-zoom-oauth.md) | Inspect the repo, choose the right Zoom OAuth flow, and wire the auth path cleanly |
| [`/setup-zoom-webhooks`](commands/setup-zoom-webhooks.md) | Implement or correct a Zoom webhook receiver with validation, signature checks, and reliable delivery handling |
| [`/setup-zoom-websockets`](commands/setup-zoom-websockets.md) | Implement or correct a Zoom WebSocket event stream with connection lifecycle and reconnect handling |
| [`/debug-zoom-auth`](commands/debug-zoom-auth.md) | Isolate OAuth, SDK auth, or token lifecycle failures and propose the minimal fix |
| [`/debug-zoom-webhook`](commands/debug-zoom-webhook.md) | Triage webhook registration, signature validation, delivery, and handler issues |
| [`/zoom-integration-doctor`](commands/zoom-integration-doctor.md) | Run a read-first integration audit across auth, SDK/API choice, and eventing |

## Build Commands

Use the bundled build commands when you want Codex to drive a specific Zoom implementation path:

| Command | Description |
|---|---|
| [`/build-zoom-rest-api-app`](commands/build-zoom-rest-api-app.md) | Implement a Zoom REST API integration with the right resources, auth path, and verification loop |
| [`/build-zoom-apps-sdk-app`](commands/build-zoom-apps-sdk-app.md) | Implement a Zoom Apps SDK app that runs inside the Zoom client with the right running context and auth path |
| [`/build-zoom-meeting-app`](commands/build-zoom-meeting-app.md) | Implement an embedded or managed Zoom meeting flow in the current codebase |
| [`/build-zoom-meeting-sdk-app`](commands/build-zoom-meeting-sdk-app.md) | Implement a Zoom Meeting SDK integration with the right platform-specific join or start flow |
| [`/build-zoom-video-sdk-app`](commands/build-zoom-video-sdk-app.md) | Implement a custom Zoom Video SDK session workflow |
| [`/build-zoom-ui-toolkit-app`](commands/build-zoom-ui-toolkit-app.md) | Implement a Zoom Video SDK UI Toolkit integration for a prebuilt web session UI |
| [`/build-zoom-cobrowse-app`](commands/build-zoom-cobrowse-app.md) | Implement a Zoom Cobrowse integration with session lifecycle, privacy controls, and support workflow wiring |
| [`/build-zoom-rivet-app`](commands/build-zoom-rivet-app.md) | Implement a server-side Zoom integration with Rivet modules for auth, APIs, and webhooks |
| [`/build-zoom-probe-flow`](commands/build-zoom-probe-flow.md) | Implement readiness checks with Zoom Probe SDK before users join meetings or sessions |
| [`/build-zoom-rtms-app`](commands/build-zoom-rtms-app.md) | Implement a Zoom RTMS workflow for live media, transcript, or event-stream processing |
| [`/build-zoom-scribe-app`](commands/build-zoom-scribe-app.md) | Implement a Zoom Scribe transcription pipeline for uploaded or stored media |
| [`/build-zoom-bot`](commands/build-zoom-bot.md) | Implement a Zoom meeting bot, recorder, or real-time media workflow |
| [`/build-zoom-team-chat-app`](commands/build-zoom-team-chat-app.md) | Implement a Zoom Team Chat integration or chatbot flow |
| [`/build-zoom-phone-integration`](commands/build-zoom-phone-integration.md) | Implement a Zoom Phone integration around APIs, Smart Embed, or events |
| [`/build-zoom-contact-center-app`](commands/build-zoom-contact-center-app.md) | Implement a Zoom Contact Center integration for web, mobile, or backend workflows |
| [`/build-zoom-virtual-agent`](commands/build-zoom-virtual-agent.md) | Implement a Zoom Virtual Agent integration for web or mobile wrappers |

## Marketplace Helper MCP

The plugin can plan and validate Marketplace app payloads without an MCP server. To let Codex
create or validate apps programmatically, connect the separate helper MCP server first. The plugin
does not bundle or host that helper.

Replace `YOUR_HELPER_HOST` with the helper's externally reachable HTTPS host, such as a hosted
service or temporary tunnel. The helper must expose `/mcp`:

```bash
codex mcp add zoom-marketplace-helper \
  --url https://YOUR_HELPER_HOST/mcp \
  --oauth-resource https://YOUR_HELPER_HOST/mcp

codex mcp login zoom-marketplace-helper \
  --scopes marketplace:read,marketplace:write,offline_access
```

The listed scopes are defined by the helper MCP server's OAuth provider; they are not Zoom
Marketplace app scopes. Run `codex mcp list` after login, then use
[`/setup-zoom-marketplace-app`](commands/setup-zoom-marketplace-app.md).

### Testing Local App URLs

If you are creating and testing a Zoom app without an existing deployed server, expose your
local app over HTTPS with [ngrok](https://ngrok.com/) or, alternatively,
[Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/):

```bash
ngrok http YOUR_LOCAL_PORT
# or
cloudflared tunnel --url http://localhost:YOUR_LOCAL_PORT
```

Use the generated HTTPS URL when configuring the app's home URL, OAuth redirect URL, and
webhook endpoint. After the tunnel is running, use the configured `zoom-marketplace-helper`
MCP server to create the app with those URLs or update the existing app by its `app_id`.
Register the exact callback paths in Zoom Marketplace, keep the tunnel running while testing,
and repeat the helper update if the tunnel URL changes. Use a deployed service for production
rather than a temporary tunnel.

## Reviewer Agents

The plugin also bundles focused reviewer agents for specialist analysis:

| Agent | Description |
|---|---|
| [`zoom-oauth-scope-auditor`](agents/zoom-oauth-scope-auditor.md) | Review scope sets for least privilege, missing scopes, and cross-surface mistakes |
| [`zoom-integration-reviewer`](agents/zoom-integration-reviewer.md) | Review a Zoom integration end to end for architectural, auth, webhook, and SDK risks |

## Primary Workflows

Codex can invoke skills implicitly from task descriptions, or explicitly by mentioning a skill such as `$start` or `$setup-zoom-oauth`.

| Skill | Description |
|---|---|
| [`start`](skills/start/SKILL.md) | Start with a Zoom app idea and route to the right product and build path |
| [`setup-zoom-marketplace-app`](skills/setup-zoom-marketplace-app/SKILL.md) | Select, create, update, or validate the Marketplace app before product implementation |
| [`setup-zoom-oauth`](skills/setup-zoom-oauth/SKILL.md) | Choose the auth model, scopes, and redirect flow for a Zoom app |
| [`build-zoom-meeting-app`](skills/build-zoom-meeting-app/SKILL.md) | Build an embedded or managed Zoom meeting flow |
| [`build-zoom-bot`](skills/build-zoom-bot/SKILL.md) | Build bots, recorders, and real-time meeting processors |
| [`debug-zoom`](skills/debug-zoom/SKILL.md) | Triage a broken Zoom integration and isolate the failing layer |
| [`zoom-mcp`](skills/zoom-mcp/SKILL.md) | Select the correct Zoom-hosted MCP server, endpoint, tools, and Marketplace template |
| [`build-zoom-rest-api-app`](skills/rest-api/SKILL.md) | Route into Zoom REST endpoints, scopes, and resource patterns |
| [`build-zoom-meeting-sdk-app`](skills/meeting-sdk/SKILL.md) | Route into embedded Zoom meeting implementation details |
| [`build-zoom-video-sdk-app`](skills/video-sdk/SKILL.md) | Route into custom video-session implementation details |
| [`build-zoom-rtms-app`](skills/rtms/SKILL.md) | Route into Zoom RTMS for live media, transcript, and event-stream workflows |
| [`setup-zoom-webhooks`](skills/webhooks/SKILL.md) | Set up Zoom webhook subscriptions, signature verification, and handlers |
| [`setup-zoom-websockets`](skills/websockets/SKILL.md) | Set up Zoom WebSocket event delivery when it fits better than webhooks |
| [`scribe`](skills/scribe/SKILL.md) | Build AI Services transcription pipelines for uploaded or stored media |
| [`summarizer`](skills/summarizer/SKILL.md) | Build AI Services transcript recap, summary, and action item workflows |
| [`translator`](skills/translator/SKILL.md) | Build AI Services text translation and localization workflows |
| [`build-zoom-team-chat-app`](skills/team-chat/SKILL.md) | Build Team Chat user or chatbot integrations |
| [`build-zoom-phone-integration`](skills/phone/SKILL.md) | Build Zoom Phone integrations around Smart Embed, APIs, and events |
| [`build-zoom-contact-center-app`](skills/contact-center/SKILL.md) | Build Contact Center app, web, or native integrations |
| [`build-zoom-virtual-agent`](skills/virtual-agent/SKILL.md) | Build Virtual Agent web or mobile wrapper integrations |

## Supporting References

The plugin keeps the Zoom product-specific reference library under `skills/`. These are supporting references, not the primary entry surface:

- [`skills/general/`](skills/general/)
- [`skills/rest-api/`](skills/rest-api/)
- [`skills/meeting-sdk/`](skills/meeting-sdk/)
- [`skills/video-sdk/`](skills/video-sdk/)
- [`skills/webhooks/`](skills/webhooks/)
- [`skills/websockets/`](skills/websockets/)
- [`skills/rtms/`](skills/rtms/)
- [`skills/oauth/`](skills/oauth/)
- [`skills/zoom-mcp/`](skills/zoom-mcp/)
- [`skills/scribe/`](skills/scribe/)
- [`skills/summarizer/`](skills/summarizer/)
- [`skills/translator/`](skills/translator/)

## Example Prompts

```text
Search my recent Zoom meetings for the discussion about SDK migration, then use the findings to plan the implementation work.
```

```text
Use $start to plan an internal meeting assistant that extracts action items and stores summaries.
```

```text
Run /build-zoom-meeting-app to add a Zoom join flow to this React app with the right auth and server-side pieces.
```

```text
Run /debug-zoom-webhook to diagnose why Zoom events reach the endpoint but signature validation fails.
```

```text
Run /plan-zoom-product for an internal meeting assistant that needs summaries, action items, and follow-up docs so we pick the right Zoom surface first.
```

```text
Use $summarizer and $translator to turn a transcript into action items and localized follow-up notes.
```
