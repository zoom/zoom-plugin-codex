# Zoom AI Services Scribe API Reference

Canonical sources:
- OpenAPI JSON: https://developers.zoom.us/api-hub/ai-services/methods/endpoints.json
- Docs overview: https://developers.zoom.us/docs/ai-services/scribe/
- Live Mode: https://developers.zoom.us/docs/ai-services/scribe/live-mode/
- Base URL: `https://api.zoom.us/v2`

## Endpoint Inventory

| Method | Endpoint | Summary | Operation ID |
|--------|----------|---------|-------------|
| WebSocket | `wss://api.zoom.us/v2/aiservices/scribe/live` | Stream PCM16 audio and receive real-time transcription events | Not represented as a REST operation |
| POST | `/aiservices/scribe/transcribe` | Scribe (Synchronous) | `createFastAsr` |
| POST | `/aiservices/scribe/jobs` | Submit Batch Scribe Job | `submitBatchAsr` |
| GET | `/aiservices/scribe/jobs` | List Batch Jobs | `listBatchJobs` |
| GET | `/aiservices/scribe/jobs/{jobId}` | Get Batch Job Status | `getBatchJobStatus` |
| DELETE | `/aiservices/scribe/jobs/{jobId}` | Cancel Batch Job | `cancelBatchJob` |
| GET | `/aiservices/scribe/jobs/{jobId}/files` | List Batch Job Files | `listBatchJobFiles` |
| GET | `/aiservices/scribe/jobs/{jobId}/files/{fileId}` | Get Batch Scribe Job File | `getBatchScribeJobFile` |

## Live Mode Contract

Connection requirements:
- WebSocket subprotocol: `live-asr`
- header: `Authorization: Bearer <Build-platform JWT>`
- connect from a trusted backend; browser WebSocket clients cannot set this header

Client messages:
- `session.update`: JSON text with `language` and `audio.format` set to `pcm16`
- audio frames: binary little-endian PCM16, 16 kHz, mono, approximately 100 ms per frame
- `session.close`: JSON text requesting graceful finalization

Server events:
- `session.created`: includes `session_id`
- `session.updated`
- `input_audio_buffer.speech_started`: includes `item_id` and `audio_start_ms`
- `input_audio_buffer.speech_stopped`: includes `item_id` and `audio_end_ms`
- `transcription.completed`: includes the transcript, timing, and transcription latency
- `error`: includes `code`, `message`, and `fatal`
- `session.closed`: includes `reason`

Documented defaults:
- maximum session duration: `60 minutes`
- idle timeout: `30 seconds` without audio
- concurrent sessions: `20` default, `100` Enterprise, `1,000` Large Contact Center, and
  `5,000+` custom

## Request Shapes

### `POST /aiservices/scribe/transcribe`

Required top-level fields:
- `file`
- `config`

Common config fields:
- `language`
- `word_time_offsets`
- `channel_separation`
- `timestamps`
- `output_format`
- `profanity_filter`
- `diarization`

Response keys:
- `request_id`
- `duration_sec`
- `model`
- `result`

### `POST /aiservices/scribe/jobs`

Required top-level fields:
- `input`
- `output`
- `config`

Input subfields:
- `mode` (`SINGLE`, `PREFIX`, `MANIFEST`)
- `source` (`S3` in current spec)
- `uri`
- `manifest`
- `filters.include_globs`
- `filters.exclude_globs`
- `auth.aws.access_key_id`
- `auth.aws.secret_access_key`
- `auth.aws.session_token`

Output subfields:
- `destination`
- `uri`
- `layout` (`SINGLE`, `PREFIX`, `ADJACENT`)
- `auth.aws.*`

Config subfields:
- `language`
- `word_time_offsets`
- `channel_separation`
- `diarization`
- `profanity_filter`
- `output_format`
- `segmentation_mode`

Optional:
- `reference_id`
- `notifications.webhook_url`
- `notifications.secret`

Response keys:
- `job_id`
- `state`
- `submitted_at`

### `GET /aiservices/scribe/jobs`

Query params:
- `state`
- `page_size`
- `next_page_token`

Response keys:
- `jobs`
- `next_page_token`

### `GET /aiservices/scribe/jobs/{jobId}`

Path params:
- `jobId`

Response keys:
- `job_id`
- `state`
- `submitted_at`
- `summary`

### `GET /aiservices/scribe/jobs/{jobId}/files`

Path params:
- `jobId`

Query params:
- `page_size`
- `next_page_token`

Response keys:
- `files`
- `next_page_token`

### `GET /aiservices/scribe/jobs/{jobId}/files/{fileId}`

Path params:
- `jobId`
- `fileId`

Response keys:
- `file_id`
- `input_uri`
- `output_uri`
- `state`
- `duration_sec`
- `error`

## Current Limits and Constraints Observed in Sources

- Batch manifest max: `1000` file URIs.
- `include_globs` max items: `10`.
- `exclude_globs` max items: `10`.
- Audio/media formats called out in docs: `WAV`, `MP3`, `M4A`, `MP4`.
- Batch job rate limit label in the OpenAPI description: `LIGHT`.
