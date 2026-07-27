---
name: build-zoom-contact-center-app
description: Use when using Contact Center.
---

# Build Zoom Contact Center App

Use this skill when the integration targets Zoom Contact Center rather than general meetings, chat, or phone. Route to the platform-specific skill once the client surface is clear.

## Workflow

1. Identify the channel and client: web, Android, iOS, campaign embed, video engagement, chat engagement, Virtual Agent handoff, or scheduled callback.
2. Select or create the General or S2S Marketplace app and verify Contact Center scopes, product surface, events, and credential response shape.
3. Confirm the lifecycle: initialization, engagement start, context retrieval, state changes, transfer or handoff, and cleanup.
4. Choose the platform reference before coding; Contact Center SDKs differ by event names, lifecycle hooks, and wrapper requirements.
5. Treat engagement context as shared state and validate how it flows into CRM, ticketing, analytics, or AI workflows.
6. Debug version drift by checking SDK version, documented event names, app settings, campaign configuration, and browser or mobile lifecycle behavior.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- Web SDK: [web/SKILL.md](web/SKILL.md)
- Android SDK: [android/SKILL.md](android/SKILL.md)
- iOS SDK: [ios/SKILL.md](ios/SKILL.md)
- High-level scenarios: [scenarios/high-level-scenarios.md](scenarios/high-level-scenarios.md)
- Common drift and breaks: [troubleshooting/common-drift-and-breaks.md](troubleshooting/common-drift-and-breaks.md)
