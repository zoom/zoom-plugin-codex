---
name: rivet-sdk
description: Use when using Rivet SDK.
---

# Zoom Rivet SDK

Use this skill when building a server-side Zoom integration with Rivet rather than hand-rolled API and webhook plumbing.

## Workflow

1. Confirm Rivet is the right abstraction for the integration’s REST, webhook, auth, and deployment needs.
2. Create or validate the General, S2S, webhook/event, or Video SDK app credentials required by the selected Rivet modules.
3. Model the modules: app configuration, OAuth, API clients, webhook handlers, and business workflow handlers.
4. Implement the smallest authenticated API call and webhook receiver before composing multi-module flows.
5. Keep deployment constraints explicit, especially Lambda-style receivers and environment variable handling.
6. Debug by checking app credentials, token refresh, webhook signature handling, module wiring, and framework version drift.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace app management: [../rest-api/references/marketplace-apps.md](../rest-api/references/marketplace-apps.md)
- Rivet overview: [rivet-sdk.md](rivet-sdk.md)
- Architecture and lifecycle: [concepts/architecture-and-lifecycle.md](concepts/architecture-and-lifecycle.md)
- Getting started pattern: [examples/getting-started-pattern.md](examples/getting-started-pattern.md)
- Multi-client pattern: [examples/multi-client-pattern.md](examples/multi-client-pattern.md)
- Common issues: [troubleshooting/common-issues.md](troubleshooting/common-issues.md)
