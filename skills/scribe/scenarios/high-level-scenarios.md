# High-Level Scenarios

## Scenario 1: On-Demand Upload Transcription

Use fast mode when a user uploads one file and expects a transcript immediately.

Flow:
1. Browser uploads file to your backend.
2. Backend generates Build JWT.
3. Backend calls `POST /aiservices/scribe/transcribe`.
4. Backend returns transcript JSON to the caller.

Common downstream uses:
- post-call summaries
- ticket enrichment
- searchable clip libraries
- internal review or handoff notes

## Scenario 2: Batch S3 Archive Transcription

Use batch mode when call archives or media libraries already live in S3.

Flow:
1. Build a batch request with input prefix and output prefix.
2. Submit `POST /aiservices/scribe/jobs`.
3. Track state by webhook or polling.
4. Read `/jobs/{jobId}/files` for per-file success/failure.
5. Ingest outputs into search, analytics, or storage.

Common downstream uses:
- compliance and audit logging
- searchable webinar or podcast archives
- bulk transcript backfills
- QA scoring inputs

## Scenario 3: Zoom Recording Export + Re-Transcription

Use when you must re-process Zoom-managed recordings with your own transcript settings.

Skill chain:
- `zoom-rest-api` to fetch/download recordings
- `scribe` to transcribe exported media

Typical reasons:
- you need your own retention/search pipeline
- you need different transcript settings than Zoom-managed defaults
- you want to enrich recordings with your own summarization or tagging flow

## Scenario 4: Compliance / QA Processing

Use batch mode when transcripts must be generated offline for audits, QA scoring, or archival search.

Prefer:
- `word_time_offsets=true` when reviewers need precise excerpts
- `channel_separation=true` for stereo call recordings
- webhook + queue ingestion instead of synchronous polling for large volumes

## Scenario 5: Customer Support Voice-to-Insights Pipeline

Use when support call recordings should feed operational analytics instead of stopping at raw transcript text.

Flow:
1. Ingest call recordings from storage or exported meeting assets.
2. Transcribe with `scribe`.
3. Store transcript plus speaker/timing metadata.
4. Run downstream sentiment, keyword, escalation, or QA logic in your own pipeline.

Guardrail:
- keep `scribe` focused on transcription
- do sentiment analysis, keyword detection, or scoring in downstream services after transcript generation

## Scenario 6: Live Microphone or Voice-Agent Transcription

Use Live Mode when an application owns a microphone, voice-agent, or telephony audio stream and
needs completed transcript segments after each detected speech turn.

Flow:
1. A trusted backend opens `wss://api.zoom.us/v2/aiservices/scribe/live` with the `live-asr`
   subprotocol and a Build-platform JWT.
2. Send `session.update` with the language and PCM16 format.
3. Stream little-endian, 16 kHz, mono PCM16 in approximately 100 ms binary frames.
4. Consume speech events and persist `transcription.completed` segments.
5. Send `session.close`, drain final events through `session.closed`, and close the socket.

For browser microphone capture, convert audio with an `AudioWorklet` and relay it through an
authenticated backend WebSocket. Never place the Build-platform JWT in browser code.

Guardrail:
- Live Mode transcribes audio the application already owns; it does not acquire meeting media
- use RTMS when the source is a Zoom meeting, webinar, Video SDK session, or Contact Center stream
