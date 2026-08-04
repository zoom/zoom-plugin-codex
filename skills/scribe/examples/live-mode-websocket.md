# Live Mode WebSocket Example

Use Live Mode for real-time transcription of app-owned audio such as a microphone, voice agent,
or telephony stream. Connect from a trusted backend so the Build-platform JWT never reaches the
browser.

## Backend Connection

```js
import WebSocket from 'ws';

const zoom = new WebSocket(
  'wss://api.zoom.us/v2/aiservices/scribe/live',
  ['live-asr'],
  {
    headers: {
      Authorization: `Bearer ${generateBuildJwt()}`,
    },
  },
);

zoom.on('open', () => {
  zoom.send(JSON.stringify({
    type: 'session.update',
    language: 'en-US',
    audio: { format: 'pcm16' },
  }));
});

zoom.on('message', (payload, isBinary) => {
  if (isBinary) return;
  const event = JSON.parse(payload.toString());

  if (event.type === 'transcription.completed') {
    console.log(event.transcript, event.transcription_latency_ms);
  } else if (event.type === 'error') {
    console.error(event.error.code, event.error.message, event.error.fatal);
  }
});

export function sendPcmFrame(frame) {
  if (zoom.readyState === WebSocket.OPEN) zoom.send(frame, { binary: true });
}

export function closeSession() {
  if (zoom.readyState === WebSocket.OPEN) {
    zoom.send(JSON.stringify({ type: 'session.close' }));
  }
}
```

`frame` must contain little-endian, 16 kHz, mono PCM16 audio. Send approximately 100 ms per
binary frame, or 1,600 samples / 3,200 bytes. Keep `session.update` and `session.close` as JSON
text messages; do not Base64-encode audio or wrap it in JSON.

After sending `session.close`, continue reading until the remaining final transcription events and
`session.closed` arrive, then close the socket.

## Browser Capture

Browsers cannot attach the required `Authorization` header to a WebSocket connection. Use this
architecture:

```text
browser microphone
  -> AudioWorklet: Float32 to 16 kHz mono PCM16
  -> your authenticated WebSocket relay
  -> Zoom Scribe Live Mode with backend-generated JWT
  -> JSON events relayed to the browser
```

Protect the relay with the application's own user/session authentication. Do not expose an open
proxy that lets anonymous callers consume the account's Scribe concurrency and usage.

See the maintained relay and AudioWorklet implementation in the
[Zoom AI Services quickstart](https://github.com/zoom/ai-services-quickstart).
