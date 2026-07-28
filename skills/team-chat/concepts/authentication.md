# Authentication Flows (Team Chat vs Chatbot)

Zoom Team Chat integrations commonly use one of two auth models:

## Team Chat API (user-level)

Use **User OAuth (authorization code)** when you want messages/actions to appear as a user.

- Typical endpoints:
  - Send message (as the user): `POST /v2/chat/users/me/messages`
- Typical scopes:
  - `chat_message:write`
  - `chat_channel:read` (for listing channels)

## Chatbot API (bot-level)

Use **client credentials** when you want messages/actions to appear as a bot.

- Typical endpoint:
  - Send bot message: `POST /v2/im/chat/messages`
- Token grant: `grant_type=client_credentials`
- Do not use authorization-code or user OAuth tokens for chatbot messages.
- Typical “scope”:
  - `imchat:bot` (added by enabling Chatbot feature on the app)

Build the outgoing payload from the `bot_notification` event. The bot-message request must
include the configured `robot_jid`, the event's `toJid`, `userJid`, and `accountId` as
`robot_jid`, `to_jid`, `user_jid`, and `account_id`.

## Decision Checklist

- If you need to post to a channel “as a bot” and handle slash command interactions: use **Chatbot API**.
- If you need to post “as the user” (and respect the user’s channel membership): use **Team Chat API**.

## Common Pitfalls

- **Server-to-Server OAuth** is not a fit for Zoom Team Chat chatbot features.
- Team Chat API calls require a user token with the right scopes; “invalid access token” errors are almost always missing scopes or wrong app type.
- OAuth URL split is easy to mix up:
  - authorize step: `https://zoom.us/oauth/authorize`
  - token step (all grant types): `https://zoom.us/oauth/token`
- A webhook HTTP 200 only acknowledges event receipt. Inspect and log the outbound
  `/v2/im/chat/messages` status/body and confirm the reply is visible in Team Chat.
- In browser demos, complete OAuth end-to-end in app (state verify -> callback -> code exchange -> token store) to avoid copy/paste mistakes.
