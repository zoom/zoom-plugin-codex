# Samples Validation

Validated against:
- https://github.com/zoom/ai-services-quickstart/
- official docs pages under `docs/ai-services/`
- AI Services OpenAPI inventory at `api-hub/ai-services/methods/endpoints.json`
- Zoom blog context:
  - `introducing-zoom-ai-services`
  - `voice-insights-modernize-customer-support-with-scribe`

## What the official quickstart confirms

- Node/Express proxy architecture is a valid implementation model.
- The current official quickstart covers Scribe, Summarizer, and Translator from one AI Services app.
- Scribe Live Mode uses a backend WebSocket relay at `/live/scribe`; the relay opens Zoom's
  WebSocket with a fresh JWT and forwards binary audio and JSON events in both directions.
- The browser sample uses an `AudioWorklet` to convert microphone Float32 samples into 16 kHz
  mono PCM16 frames of approximately 100 ms.
- The browser waits for a relay-ready signal before treating recording as active, sends
  `session.close` on shutdown, and handles final, delta, error, and session-closed events.
- The current Fast Mode playground converts uploaded or recorded media to a data URI and sends it
  through the JSON `file` field. Treat this as a quickstart transport choice, not a requirement for
  every backend.
- Batch mode commonly injects AWS credentials into request payloads.
- Webhook verification uses `x-zm-signature` + `x-zm-request-timestamp` with HMAC-SHA256 and `sha256=` prefix.
- The quickstart uses `ZOOM_API_KEY` / `ZOOM_API_SECRET` naming.

## Useful implementation details from the sample

- A tRPC/Express backend can keep Build credentials server-side while accepting a data URI from a
  small Fast Mode demo.
- Batch helper routes are naturally expressed as:
  - `POST /batch/jobs`
  - `GET /batch/jobs`
  - `GET /batch/jobs/:jobId`
  - `GET /batch/jobs/:jobId/files`
  - `DELETE /batch/jobs/:jobId`
- It is practical to keep one `generateJWT()` helper and inject the bearer token per request.
- Buffer browser frames only until the upstream Zoom WebSocket opens; bound that queue in a
  production relay and apply application authentication and usage controls before accepting audio.

## Caveats from the sample

- The README setup command still says to clone `zoom/scribe-quickstart.git`; the current repo name is `zoom/ai-services-quickstart.git`.
- It assumes Node `>=24`, which is stricter than many deployment environments actually need. Verify your runtime before copying that constraint unchanged.
- It uses environment-injected AWS credentials. Production pipelines may prefer pre-signed URLs or short-lived STS credentials only.
- Data URIs increase request size and memory pressure; prefer URL-based input, bounded uploads, or
  another validated transport for larger files.
- The sample is an app demo, not a complete production reference for job retry policy, durable queues, or transcript storage.
- The Live relay is intentionally compact. Production implementations still need user/session
  authorization, connection quotas, bounded buffering, request correlation, metrics, and abuse
  protection.

## What the blog posts add

- They reinforce the highest-value downstream use cases:
  - post-call summaries
  - ticket enrichment
  - compliance/audit logging
  - searchable archives
  - customer-support QA workflows
- They are useful for scenario framing, but not as authoritative API surface documentation.
- Keep endpoint and request-shape decisions anchored to the AI Services docs and API Hub inventory, not the blog wording.
