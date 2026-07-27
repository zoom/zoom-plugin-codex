---
name: zoom-rtms
description: Use when using RTMS.
---

# Zoom Realtime Media Streams

Use this skill when the integration needs real-time meeting or contact-center media as a backend stream. If the workflow needs a visible participant bot, compare against `build-zoom-bot` and `zoom-meeting-sdk-linux` before choosing RTMS.

## Workflow

1. Confirm the source surface: Meetings RTMS, Contact Center voice media, or another documented RTMS stream.
2. Select the correct Marketplace model: user-managed General App for Meeting/Webinar RTMS, General or S2S for Contact Center Voice, or Video SDK credentials for Video SDK RTMS.
3. Decide whether RTMS is sufficient or whether a Meeting SDK bot is required for participant identity, meeting controls, or UI-visible behavior.
4. Implement the WebSocket lifecycle first: event subscription, connection validation, media start, heartbeat, reconnect, and shutdown.
5. Process media types intentionally: audio, video, screen share, chat, live transcript, and metadata have different payload and timing constraints.
6. Design downstream pipelines for buffering, transcription, AI analysis, recording, or tool invocation.
7. Debug by isolating app setup, webhook/event delivery, stream authorization, network connectivity, and media payload decoding.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Marketplace template selector: [../rest-api/references/marketplace-app-templates.md](../rest-api/references/marketplace-app-templates.md)
- Connection architecture: [concepts/connection-architecture.md](concepts/connection-architecture.md)
- Lifecycle flow: [concepts/lifecycle-flow.md](concepts/lifecycle-flow.md)
- Media types: [references/media-types.md](references/media-types.md)
- Data types: [references/data-types.md](references/data-types.md)
- RTMS bot example: [examples/rtms-bot.md](examples/rtms-bot.md)
- Common issues: [troubleshooting/common-issues.md](troubleshooting/common-issues.md)
