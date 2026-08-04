---
name: scribe
description: Use when building or debugging Zoom AI Services Scribe transcription in Live, Fast, or Batch mode, including secure WebSocket PCM16 streams, individual files, and stored-media jobs.
---

# Zoom AI Services Scribe

Use this skill for Zoom Scribe transcription pipelines over live interactive audio, uploaded files, or stored media. Use Scribe Live Mode for app-owned microphone or voice-agent streams. If the source is a Zoom meeting, webinar, or Video SDK session, compare against RTMS or a Meeting SDK bot first. Chain Summarizer or Translator for downstream processing.

## Workflow

1. Confirm the media source, language, expected latency, and whether Live, Fast, or Batch mode fits.
2. Set up Build-platform credentials and JWT handling separately from application transcription logic.
3. For Live Mode, keep the JWT on a trusted backend, configure the WebSocket session, and stream binary PCM16 frames through a backend connection or browser relay.
4. For Fast or Batch mode, design file ingestion, retries, webhook callbacks, and persistence before connecting downstream AI workflows.
5. Implement the smallest working transcription flow and response/event parser, then add operational monitoring.
6. Debug by checking credential audience, mode, audio format, session lifecycle, file size, backend timeout, and webhook delivery.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Auth and processing modes: [concepts/auth-and-processing-modes.md](concepts/auth-and-processing-modes.md)
- High-level scenarios: [scenarios/high-level-scenarios.md](scenarios/high-level-scenarios.md)
- Live Mode WebSocket example: [examples/live-mode-websocket.md](examples/live-mode-websocket.md)
- Fast mode Node example: [examples/fast-mode-node.md](examples/fast-mode-node.md)
- Batch webhook pipeline: [examples/batch-webhook-pipeline.md](examples/batch-webhook-pipeline.md)
- API reference: [references/api-reference.md](references/api-reference.md)
- Common drift and breaks: [troubleshooting/common-drift-and-breaks.md](troubleshooting/common-drift-and-breaks.md)
- Chain summaries: [../summarizer/SKILL.md](../summarizer/SKILL.md)
- Chain translations: [../translator/SKILL.md](../translator/SKILL.md)
