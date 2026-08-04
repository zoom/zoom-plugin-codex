# Scribe 5-Minute Preflight Runbook

Use this before deep debugging.

## 1) Confirm the Right Product

- App-owned microphone, voice-agent, or telephony audio -> use Scribe Live Mode.
- File-based or storage-based transcription -> use Scribe Fast or Batch mode.
- Media from a Zoom meeting, webinar, Video SDK session, or Contact Center engagement -> use
  `rtms` instead.
- Meeting bot that joins and records before transcription -> chain Meeting SDK Linux first.

## 2) Confirm Credentials

- Build-platform issuer credential pair available.
- JWT generation uses `HS256` with one-hour-or-less expiry.
- Secret stays server-side.
- Reject placeholder values such as `${ZOOM_API_KEY}` and `${ZOOM_API_SECRET}`. They can make a naive health check look configured while every real call still fails.

## 3) Confirm Mode Selection

- **Live Mode** for app-owned continuous audio and completed speech-turn transcripts.
- **Fast mode** for one short file and immediate JSON response.
- **Batch mode** for many files, long recordings, or archive-style processing.
- Live Mode requirements:
  - backend WebSocket to `wss://api.zoom.us/v2/aiservices/scribe/live`
  - `live-asr` subprotocol and Build-platform JWT bearer header
  - binary little-endian, 16 kHz, mono PCM16 in approximately 100 ms frames
  - JSON `session.update` before audio and graceful `session.close` at shutdown
  - default `60-minute` maximum session and `30-second` audio idle timeout
- Browser microphone capture must use an authenticated backend relay because browser WebSocket
  clients cannot set the Zoom authorization header.
- Fast mode current limits from the API spec:
  - maximum file size: `100 MB`
  - maximum duration: `2 hours`
- If fast mode is exposed through a hosted browser UI, prefer an async wrapper:
  - browser uploads once
  - backend returns `202` with a request ID
  - frontend polls for completion
  This avoids losing successful transcriptions to edge/client timeout races.
- Observed hosted timing from the deployed sample:
  - ~17.2 MB MP4 completed in ~26s
  - ~38.6 MB MP4 completed in ~26-37s
  - ~59.2 MB MP4 completed in ~32-34s on the backend
  - some ~59.2 MB requests still surfaced as frontend `504` even though the backend later completed with `200`
  Treat these as deployment observations, not hard API guarantees.

## 4) Confirm Storage / Webhook Inputs

- Fast mode file URL or upload path resolves.
- Batch input/output URIs are valid.
- AWS or pre-signed access is set correctly for S3 mode.
- Webhook URL is public HTTPS if you expect notifications.

## 5) Confirm Post-Processing Contract

- Decide whether downstream code expects `text_display`, segments, or word-level timings.
- Decide whether channel separation or diarization is required before shipping.

## 6) Quick Probes

- JWT generation works locally.
- `POST /aiservices/scribe/transcribe` succeeds with a known small file.
- For normal browser-uploaded files, backend forwarding should use `multipart/form-data` to Zoom.
- Live Mode connects from the backend, acknowledges session creation/update, accepts one known PCM16
  sample, returns `transcription.completed`, and reaches `session.closed` after a graceful close.
- Batch submit returns `201` with `job_id`.
- Webhook signature verification works with the configured secret.

## 7) Fast Decision Tree

- `401`/auth failure -> wrong credential pair or expired JWT.
- Live handshake `401`/`403` -> wrong Build-platform credentials, expired JWT, or missing access.
- Live handshake `404` -> verify AI Services provisioning and the exact WebSocket endpoint.
- Live handshake `429` -> account concurrency or rate limit; inspect active sessions and retry policy.
- Live session connects but returns no speech events -> verify binary PCM16, little-endian encoding,
  16 kHz mono audio, frame cadence, and that `session.update` was sent first.
- Live session closes after silence -> the default idle timeout is 30 seconds without audio.
- Fast mode returns schema error -> wrong request body or config fields.
- Fast mode returns `413 Request Entity Too Large` before the app logs anything -> reverse proxy limit, not Scribe.
- Frontend returns `504` but backend logs later show `200` -> browser/edge timeout race; poll by request ID instead of assuming failure.
- Browser cannot connect directly with auth -> add a trusted backend WebSocket relay; never put the
  Build JWT in browser code.
- Audio must come from a Zoom meeting/webinar/Video SDK session -> use `rtms`; Live Mode does not
  acquire Zoom session media.
- Batch jobs queue but never complete -> storage auth / URI / webhook issues.
- Missing transcripts for some files -> inspect `/jobs/{jobId}/files` before re-submitting whole batch.

## 8) Source Checkpoints

### Official docs

- https://developers.zoom.us/docs/ai-services/
- https://developers.zoom.us/docs/ai-services/scribe/
- https://developers.zoom.us/docs/ai-services/scribe/live-mode/
- https://developers.zoom.us/api-hub/ai-services/methods/endpoints.json

### Raw docs in repo tooling output

- `tools/zoom-crawler/raw-docs/developers.zoom.us/docs/ai-services/`
