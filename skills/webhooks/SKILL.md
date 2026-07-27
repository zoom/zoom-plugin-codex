---
name: setup-zoom-webhooks
description: Use when building Zoom webhooks.
---

# Setup Zoom Webhooks

Use this skill when the integration receives Zoom events over HTTP. If the user needs persistent low-latency event streams, compare against `setup-zoom-websockets`.

## Workflow

1. Identify the event types and resource scope before creating subscriptions.
2. Select or create the user-managed General, admin-managed General, S2S, or Build Platform app that owns the subscription.
3. Implement endpoint verification and signature verification before processing business logic.
4. Store raw event IDs, timestamps, and delivery metadata for replay protection and debugging.
5. Make handlers idempotent because Zoom can retry delivery.
6. Separate webhook ingestion from downstream API calls with a queue when reliability matters.
7. Debug by checking endpoint reachability, TLS, validation token handling, signature base string, app event subscription, and account-level settings.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- Signature verification: [references/signature-verification.md](references/signature-verification.md)
- Subscriptions: [references/subscriptions.md](references/subscriptions.md)
- Events: [references/events.md](references/events.md)
- Common issues: [troubleshooting/common-issues.md](troubleshooting/common-issues.md)
