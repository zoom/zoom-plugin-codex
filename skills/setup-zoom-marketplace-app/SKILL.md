---
name: setup-zoom-marketplace-app
description: Use when creating or validating a Zoom Marketplace app.
---

# Setup Zoom Marketplace App

Use this skill before product implementation when the task needs app-model selection, manifest validation, scopes, event subscriptions, or app credentials.

## Workflow

1. Identify the product scenario, actor, account ownership, and required app model.
2. Select the narrowest JSON template from the Marketplace template selector.
3. Replace sample values and reduce scopes to the exact operations required.
4. Validate General App manifests and check both HTTP status and response `ok`.
5. Create the app through the correct regular or account-scoped endpoint.
6. Complete post-create event, WebSocket, RTMS, or feature setup that is not reliably encoded by the schema.
7. Store secrets safely and return to the owning product skill.

## References

- Marketplace app management: [../rest-api/references/marketplace-apps.md](../rest-api/references/marketplace-apps.md)
- Template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- OAuth: [../oauth/SKILL.md](../oauth/SKILL.md)
- REST API: [../rest-api/SKILL.md](../rest-api/SKILL.md)
