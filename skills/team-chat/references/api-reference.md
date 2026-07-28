# API Reference Pointers

This doc is intentionally lightweight; prefer the official REST reference for the authoritative schema.

## Team Chat API (user-level)

- Send message: `POST /v2/chat/users/me/messages`
- Typical needs:
  - list channels
  - post to channel / DM
  - thread replies

## Chatbot API (bot-level)

- Send bot message: `POST /v2/im/chat/messages`
- Token grant: `client_credentials` at `https://zoom.us/oauth/token`
- Do not use an authorization-code or user OAuth token for this endpoint.
- Required routing fields come from `bot_notification`: `robot_jid`, `to_jid`, `user_jid`, and `account_id`.
- A webhook HTTP 200 confirms receipt only; inspect the outbound response status/body and the visible Team Chat reply.

## Notes

- If you see "invalid access token" errors, check:
  - app type (General App OAuth vs others)
  - scopes
  - whether the user re-consented after scope changes
