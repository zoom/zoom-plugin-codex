---
name: build-zoom-team-chat-app
description: Use when building Team Chat.
---

# Build Zoom Team Chat App

Use this skill when the target surface is Zoom Team Chat. First decide whether the integration is user-scoped messaging, a chatbot, an interactive message-card workflow, or a webhook-driven automation.

## Workflow

1. Choose the API surface: Team Chat API for user-scoped actions, Chatbot API for bot identity and chatbot workflows.
2. Select the user, admin, S2S API, or chatbot Marketplace template; do not use S2S for the chatbot subscription feature.
3. Confirm app type, scopes, role enablement, and whether the account has Zoom for Developers enabled.
4. Model message structure before coding: plain messages, rich cards, buttons, dropdowns, forms, slash commands, and threaded replies.
5. For Chatbot API messages, obtain the access token with `grant_type=client_credentials`; do not use an authorization-code or user OAuth token.
6. Build `/v2/im/chat/messages` payloads from the incoming `bot_notification` fields, including `robot_jid`, `to_jid`, `user_jid`, and `account_id`.
7. Add webhook handlers for interactivity and lifecycle events with signature verification and retry-safe processing. A webhook HTTP 200 confirms receipt only, not a successful chatbot reply.
8. Verify the outbound message API response status/body and the visible Team Chat reply; test with a real slash command end to end.
9. For 401/code 7010, check for mixed development/production tokens, Bot JIDs, and Marketplace app credentials before changing payloads.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- API selection: [concepts/api-selection.md](concepts/api-selection.md)
- Message structure: [concepts/message-structure.md](concepts/message-structure.md)
- Message cards: [references/message-cards.md](references/message-cards.md)
- Scopes: [references/scopes.md](references/scopes.md)
- Chatbot setup: [examples/chatbot-setup.md](examples/chatbot-setup.md)
- Common issues: [troubleshooting/common-issues.md](troubleshooting/common-issues.md)
