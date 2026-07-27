---
name: zoom-general
description: Use when comparing products.
---

# Zoom General

Use this skill for cross-product platform context after the user’s goal is understood. For first-step routing, prefer `start`, `plan-zoom-product`, or `plan-zoom-integration`.

## Workflow

1. Classify the job by outcome: embed meetings, custom video, automate resources, consume events, process media, use meeting intelligence, translate text, summarize transcripts, or publish a Marketplace app.
2. Choose the Zoom surface that owns the behavior: REST API, webhooks, WebSockets, Meeting SDK, Video SDK, Zoom Apps SDK, Phone, Contact Center, Virtual Agent, Scribe, Summarizer, Translator, Rivet, or Cobrowse.
3. Create or validate the owning Marketplace app and manifest before implementing auth, scopes, events, or SDK signatures.
4. Confirm auth and scope model before implementation; Zoom surfaces differ on user-level OAuth, account-level OAuth, and SDK signatures.
5. Route to the narrow skill once the surface is chosen rather than keeping broad guidance in context.
6. Use the preserved guide only when a task crosses product boundaries or needs detailed comparison tables.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- App types: [references/app-types.md](references/app-types.md)
- Authentication: [references/authentication.md](references/authentication.md)
- Environment variables: [references/environment-variables.md](references/environment-variables.md)
- Scopes: [references/scopes.md](references/scopes.md)
- Marketplace: [references/marketplace.md](references/marketplace.md)
- Marketplace app management: [../rest-api/references/marketplace-apps.md](../rest-api/references/marketplace-apps.md)
- Query routing playbook: [references/query-routing-playbook.md](references/query-routing-playbook.md)
- Routing implementation: [references/routing-implementation.md](references/routing-implementation.md)
- AI Services text intelligence: [use-cases/ai-services-text-intelligence.md](use-cases/ai-services-text-intelligence.md)
