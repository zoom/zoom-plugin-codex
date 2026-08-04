# Auth and Processing Modes

## Authentication Model

Scribe uses a Build-platform JWT bearer token.

JWT shape:
- algorithm: `HS256`
- issuer claim: Build-platform credential identifier used by the Scribe API
- expiration: keep to one hour or less

Node example:

```js
import { KJUR } from 'jsrsasign';

export function generateJWT(apiKey, apiSecret) {
  const iat = Math.round(Date.now() / 1000) - 30;
  const exp = iat + 60 * 60;
  return KJUR.jws.JWS.sign(
    'HS256',
    JSON.stringify({ alg: 'HS256', typ: 'JWT' }),
    JSON.stringify({ iss: apiKey, iat, exp }),
    apiSecret,
  );
}
```

## Credential Naming Drift

Zoom docs currently use inconsistent labels across AI Services pages:
- `API key` / `API secret`
- `SDK key` / `SDK secret`
- `Build platform credentials`

For implementation, treat them as the Build-platform JWT issuer/secret pair used to sign Scribe requests. Verify the exact labels in the current portal UI before shipping.

## Live, Fast, and Batch Modes

| Mode | Best for | Transport | Result timing |
|------|----------|-----------|---------------|
| Live mode | Voice agents, live captions, app-owned microphone or telephony audio | Secure WebSocket with binary PCM16 | Completed segment after each detected speech turn |
| Fast mode | One short file, interactive UX | `POST /transcribe` | Immediate synchronous JSON |
| Batch mode | Archives, long media, many files | `POST /jobs` then status/webhook | Asynchronous |

## Live Mode Session Contract

- endpoint: `wss://api.zoom.us/v2/aiservices/scribe/live`
- WebSocket subprotocol: `live-asr`
- authentication: Build-platform JWT in `Authorization: Bearer ...`
- configuration: send `session.update` with `language` and `audio.format=pcm16`
- audio: binary little-endian PCM16, 16 kHz, mono, approximately 100 ms per frame
- control messages: JSON text; never wrap audio in JSON or Base64
- result: consume `transcription.completed` for final speech-turn transcripts
- shutdown: send `session.close`, drain final events through `session.closed`, then close

Create the Zoom WebSocket from a trusted backend. Browser WebSocket clients cannot set the
authorization header, so browser microphone capture requires an authenticated backend relay.

Default documented limits:
- maximum session duration: `60 minutes`
- idle timeout: `30 seconds` without audio
- concurrent sessions per account: `20` default, with higher published tiers

## Fast Mode Request Shape

- required: `file`, `config`
- common config: `language`, `word_time_offsets`, `channel_separation`, `timestamps`, `output_format`, `profanity_filter`, `diarization`

## Batch Mode Request Shape

- required: `input`, `output`, `config`
- input modes: `SINGLE`, `PREFIX`, `MANIFEST`
- storage provider currently surfaced in the OpenAPI as `S3`
- optional webhook callback: `notifications.webhook_url` + `notifications.secret`

## Operational Choice

Choose fast mode when:
- user uploads one file
- latency matters more than throughput
- file size and duration are manageable

Choose batch mode when:
- many files must be processed
- transcripts can arrive later
- storage-centric workflows fit better than direct upload

Choose Live mode when:
- the app owns a continuous microphone, voice-agent, or telephony audio stream
- completed transcripts are needed after each speech turn
- the backend can maintain a secure WebSocket and PCM16 framing

## Live Mode vs RTMS

Use Scribe Live Mode when the application already has access to audio it is authorized to process.
Use RTMS when the source is media or transcript data from a Zoom meeting, webinar, Video SDK
session, or Contact Center engagement. Live Mode is a transcription transport; it does not join a
meeting or grant access to Zoom session media.
