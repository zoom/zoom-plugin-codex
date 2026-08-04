---
description: Implement a Zoom Scribe Live, Fast, or Batch transcription pipeline with the right streaming, file, or callback flow.
---

# Build Zoom Scribe App

Use this command when the repo needs Zoom Scribe for interactive audio, uploaded files, or stored media. Use RTMS or a bot when the source is live media from a Zoom meeting, webinar, or Video SDK session.

## Preflight

1. Inspect the codebase for existing ingestion, storage, queueing, webhook, or transcript-processing code.
2. Confirm the media source, expected latency, language requirements, and whether Live, Fast, or Batch mode fits the workflow.
3. Identify the trusted backend that will own Build-platform credentials and any Live Mode WebSocket connection, request submission, callback, or polling behavior.
4. Check for required credentials, storage paths, WebSocket proxy paths, and callback endpoints without printing secrets.

## Plan

Before making changes:

- state the media source and chosen Scribe processing mode
- list the files that will be changed
- state the auth path, ingestion path, and transcript output destination
- state how the first transcription flow will be verified

## Commands

1. Add or correct the transcription request path in the backend or worker layer.
2. Keep Build-platform auth handling separate from transcript business logic.
3. Add the minimum ingest, submit, parse, and persist flow required for a working transcription pipeline.
4. For Live Mode, open `wss://api.zoom.us/v2/aiservices/scribe/live` from the backend with the `live-asr` subprotocol and JWT header; stream binary 16 kHz mono PCM16 frames and handle session, speech, transcription, error, and close events.
5. For browser capture, relay through the backend because browser WebSocket clients cannot attach the required authorization header.
6. If Batch mode is used, wire the callback or job-status path explicitly.
7. Reuse existing queueing, storage, and observability patterns where possible.

## Verification

1. Re-read the auth handling, submission path, and transcript parsing code after changes.
2. Run local build or tests where available.
3. Verify the chosen processing mode and transcript output path are coherent. For Live Mode, verify session creation, at least one completed transcript event, graceful session closure, and credential isolation.
4. State any remaining blocker such as credential audience, PCM framing, entitlement, session limits, callback path, or file constraints.

## Summary

```text
## Result
- Action: implemented or updated a Zoom Scribe transcription workflow
- Status: success | partial | failed
- Details: processing mode, files changed, auth path, verification run
```

## Next Steps

- Test the pipeline with one controlled audio stream or media file.
- Add retries, batching, or downstream AI processing only after the base path works.
- If the workflow needs media directly from a Zoom meeting, webinar, or Video SDK session, switch to `/build-zoom-rtms-app` or `/build-zoom-bot`.
