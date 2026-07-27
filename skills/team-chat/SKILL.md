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
5. Implement OAuth and token refresh separately from message sending, with clear storage boundaries.
6. Add webhook handlers for interactivity and lifecycle events with signature verification and retry-safe processing.
7. Debug by checking JID formats, channel membership, bot installation, scopes, role settings, and message-card payload shape.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- API selection: [concepts/api-selection.md](concepts/api-selection.md)
- Message structure: [concepts/message-structure.md](concepts/message-structure.md)
- Message cards: [references/message-cards.md](references/message-cards.md)
- Scopes: [references/scopes.md](references/scopes.md)
- Chatbot setup: [examples/chatbot-setup.md](examples/chatbot-setup.md)
- Common issues: [troubleshooting/common-issues.md](troubleshooting/common-issues.md)
