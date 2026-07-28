# Error Codes (Common Patterns)

## Auth Errors

- `Invalid access token`
  - wrong token type (bot token used for user API, or vice versa)
  - missing scopes
  - token expired / revoked
- `401` with code `7010` (`Invalid authorization token`)
  - development token sent to the production API, or production token sent to the development API
  - Bot JID belongs to a different environment
  - credentials and Bot JID belong to different Marketplace apps
  - verify the token grant and endpoint before changing the message payload

## Webhook Errors

- No events received:
  - endpoint not reachable publicly
  - verification failing
  - wrong event subscription / wrong app/account
- HTTP `200` from the webhook:
  - confirms event receipt only; inspect the outbound `/v2/im/chat/messages` response and Team Chat UI separately

## Message Rendering Issues

- Card not rendering:
  - invalid JSON payload
  - unsupported component types
