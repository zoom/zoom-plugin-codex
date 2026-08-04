# Zoom AI Services Scribe

Implementation guidance for Zoom AI Services Scribe across:
- real-time transcription over authenticated WebSocket (`wss://api.zoom.us/v2/aiservices/scribe/live`)
- synchronous single-file transcription (`POST /aiservices/scribe/transcribe`)
- asynchronous batch jobs (`/aiservices/scribe/jobs*`)
- browser microphone capture through an authenticated backend WebSocket relay
- webhook-driven batch status updates
- Build-platform JWT generation and credential handling

Official docs:
- https://developers.zoom.us/docs/ai-services/
- https://developers.zoom.us/docs/ai-services/scribe/
- https://developers.zoom.us/docs/ai-services/scribe/live-mode/
- https://developers.zoom.us/docs/api/ai-services/
- https://developers.zoom.us/api-hub/ai-services/methods/endpoints.json
- Quickstart sample: https://github.com/zoom/ai-services-quickstart/

## Routing Guardrail

- If the user needs **live interactive audio, uploaded files, or stored media transcribed into text**, route here first.
- If the user needs **transcript text summarized**, route to [../summarizer/SKILL.md](../../summarizer/SKILL.md).
- If the user needs **plain text translated**, route to [../translator/SKILL.md](../../translator/SKILL.md).
- If the user needs media from a **Zoom meeting, webinar, Video SDK session, or Contact Center engagement**, route to [../rtms/SKILL.md](../../rtms/SKILL.md).
- If the user needs **Zoom REST API inventory** for AI Services paths, chain [../rest-api/SKILL.md](../../rest-api/SKILL.md).
- If the user needs webhook signature patterns or generic HMAC receiver hardening, optionally chain [../webhooks/SKILL.md](../../webhooks/SKILL.md).

## Quick Links

1. [concepts/auth-and-processing-modes.md](../concepts/auth-and-processing-modes.md)
2. [scenarios/high-level-scenarios.md](../scenarios/high-level-scenarios.md)
3. [examples/live-mode-websocket.md](../examples/live-mode-websocket.md)
4. [examples/fast-mode-node.md](../examples/fast-mode-node.md)
5. [examples/batch-webhook-pipeline.md](../examples/batch-webhook-pipeline.md)
6. [references/api-reference.md](../references/api-reference.md)
7. [references/environment-variables.md](../references/environment-variables.md)
8. [references/samples-validation.md](../references/samples-validation.md)
9. [references/versioning-and-drift.md](../references/versioning-and-drift.md)
10. [troubleshooting/common-drift-and-breaks.md](../troubleshooting/common-drift-and-breaks.md)
11. [RUNBOOK.md](../RUNBOOK.md)

## Core Workflow

1. Get Build-platform credentials and generate an HS256 JWT.
2. Choose **Live Mode** for app-owned continuous audio, **Fast mode** for one short file, or
   **Batch mode** for stored archives and large sets.
3. Open the Live WebSocket or submit the transcription request.
4. For Live Mode, handle session, speech, transcription, error, and close events. For Batch mode,
   poll job/file status or receive webhook notifications.
5. Persist and post-process transcript JSON.

## Hosted Fast-Mode Guardrail

- The formal fast-mode API limits are `100 MB` and `2 hours`, but hosted browser flows can still time out before the upstream response returns.
- Current deployed-sample observations:
  - ~17.2 MB MP4 completed in about `26s`
  - ~38.6 MB MP4 completed in about `26-37s`
  - ~59.2 MB MP4 completed in about `32-34s` on the backend
  - some ~59.2 MB browser requests still surfaced as frontend `504` while backend logs later showed `200`
- Treat frontend `504` plus backend `200` as a browser/edge timeout race, not an automatic transcription failure.
- For hosted UIs, prefer an async request/polling wrapper for fast mode instead of holding the browser open for the full upstream response.
- For larger or less predictable media, prefer batch mode even when the file is still within the formal fast-mode size limit.

## Live Mode Pattern

- Connect to `wss://api.zoom.us/v2/aiservices/scribe/live` from a trusted backend.
- Include the `live-asr` subprotocol and Build-platform JWT bearer header.
- Send `session.update` with the language and `audio.format=pcm16`.
- Stream binary little-endian, 16 kHz, mono PCM16 in approximately 100 ms frames.
- Persist completed speech turns from `transcription.completed`.
- Send `session.close`, drain final events through `session.closed`, and then close the socket.
- For browser microphone capture, convert Float32 audio with an `AudioWorklet` and relay through
  an authenticated backend. Never expose Build credentials or the JWT to the browser.
- Use RTMS instead when the source is media from a Zoom meeting, webinar, Video SDK session, or
  Contact Center engagement.

## Endpoint Surface

| Mode | Method | Path | Use |
|------|--------|------|-----|
| Live | WebSocket | `wss://api.zoom.us/v2/aiservices/scribe/live` | Real-time transcription for app-owned PCM16 audio |
| Fast | `POST` | `/aiservices/scribe/transcribe` | Synchronous transcription for one file |
| Batch | `POST` | `/aiservices/scribe/jobs` | Submit asynchronous batch job |
| Batch | `GET` | `/aiservices/scribe/jobs` | List jobs |
| Batch | `GET` | `/aiservices/scribe/jobs/{jobId}` | Inspect job summary/state |
| Batch | `DELETE` | `/aiservices/scribe/jobs/{jobId}` | Cancel queued/processing job |
| Batch | `GET` | `/aiservices/scribe/jobs/{jobId}/files` | Inspect per-file results |
| Batch | `GET` | `/aiservices/scribe/jobs/{jobId}/files/{fileId}` | Inspect one per-file result |

## High-Level Scenarios

- On-demand clip transcription after a user uploads one recording.
- Live captions or voice-agent transcription for app-owned audio streams.
- Batch transcription of stored S3 call archives.
- Webhook-driven ETL pipeline that writes transcripts to your database/search index.
- Re-transcription of Zoom-managed recordings after exporting them to your own storage.
- Offline compliance or QA workflows that need timestamps, channel separation, and speaker hints.

## Chaining

- Stored Zoom recordings -> [../rest-api/SKILL.md](../../rest-api/SKILL.md) + `scribe`
- Transcribe then summarize -> `scribe` + [../summarizer/SKILL.md](../../summarizer/SKILL.md)
- Transcribe then translate -> `scribe` + [../translator/SKILL.md](../../translator/SKILL.md)
- Webhook verification hardening -> [../webhooks/SKILL.md](../../webhooks/SKILL.md)
- App-owned live audio transcription -> `scribe` Live Mode
- Zoom meeting/webinar/Video SDK media -> [../rtms/SKILL.md](../../rtms/SKILL.md)
- Cross-product routing -> [../general/SKILL.md](../../general/SKILL.md)

## Operations

- [RUNBOOK.md](../RUNBOOK.md) - 5-minute preflight and debugging checklist.
