# Zoom Team Chat Environment Variables

## Standard `.env` keys

| Variable | Required | Used for | Where to find |
| --- | --- | --- | --- |
| `ZOOM_CLIENT_ID` | Yes | Team Chat app OAuth identity | Zoom Marketplace -> Team Chat app -> App Credentials |
| `ZOOM_CLIENT_SECRET` | Yes | OAuth token exchange | Zoom Marketplace -> Team Chat app -> App Credentials |
| `ZOOM_REDIRECT_URI` | OAuth code flow | Callback URL for installs/auth | Zoom Marketplace -> OAuth redirect/allow list |
| `ZOOM_BOT_JID` | Chatbot flows | Target bot identifier | Team Chat app/chatbot configuration after setup |
| `ZOOM_TARGET_USER_JID` | Proactive chatbot flows | Recipient user JID when no `bot_notification` payload exists | Configure for the target Team Chat user |
| `ZOOM_SECRET_TOKEN` | Recommended | Event/webhook signature verification | Zoom Marketplace -> Event Subscriptions -> Secret Token |
| `ZOOM_VERIFICATION_TOKEN` | Legacy only | Legacy verification path | Zoom Marketplace legacy fields (older apps) |

## Runtime-only values

- `ZOOM_ACCESS_TOKEN`
- `ZOOM_REFRESH_TOKEN`

These runtime values apply to user OAuth paths. Chatbot API messages use a newly obtained
`client_credentials` token instead of a user access/refresh-token pair.

## Notes

- Prefer secret-token signature verification over legacy verification token.
