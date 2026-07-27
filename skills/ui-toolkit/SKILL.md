---
name: ui-toolkit
description: Use when using Zoom UI Toolkit.
---

# Zoom Video SDK UI Toolkit

Use this skill when the user wants a prebuilt web UI for a custom Zoom Video SDK session. If the user needs an actual Zoom meeting, route to `build-zoom-meeting-sdk-app` instead.

## Workflow

1. Confirm product fit: UI Toolkit is for Video SDK custom sessions, not Meeting SDK joins.
2. Create or verify the Video SDK Marketplace app and credential shape.
3. Set up the required Video SDK session credentials and server-side signature generation.
4. Pick the UI Toolkit integration shape and mount it inside the app shell.
5. Configure session join, leave, media permissions, chat, captions, recordings, and theming based on product requirements.
6. Keep custom UI needs explicit; if the user needs deep media layout control, route to `build-zoom-video-sdk-app`.
7. Debug browser permissions, cross-origin isolation, token generation, package version mismatch, and unsupported paid features separately.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace app management: [../rest-api/references/marketplace-apps.md](../rest-api/references/marketplace-apps.md)
- Environment variables: [references/environment-variables.md](references/environment-variables.md)
- Common issues: [troubleshooting/common-issues.md](troubleshooting/common-issues.md)
- Video SDK Web: [../video-sdk/web/SKILL.md](../video-sdk/web/SKILL.md)
