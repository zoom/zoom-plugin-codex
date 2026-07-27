---
name: build-zoom-meeting-sdk-app
description: Use when using Meeting SDK.
---

# Build Zoom Meeting SDK App

Use this skill when the user needs to join, start, or embed real Zoom meetings. If the user wants a fully custom non-meeting video experience, route to `build-zoom-video-sdk-app`.

## Workflow

1. Confirm product fit: Meeting SDK joins real Zoom meetings; Video SDK creates custom sessions; Zoom Apps SDK runs inside the Zoom client.
2. Create or verify the dedicated Meeting SDK Marketplace app before generating SDK signatures.
3. Choose the target platform: web, Android, iOS, macOS, Windows, Electron, React Native, Unreal, or Linux bot.
4. Validate the join or start path: meeting number, password, SDK signature, role, ZAK when hosting, waiting room behavior, and user identity.
5. Implement the smallest join flow first, then add meeting controls, custom UI, raw data, recording, or bot behavior.
6. Debug by isolating signature generation, SDK version, platform permissions, meeting settings, waiting room, raw data entitlement, and network constraints.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- Meeting SDK app template: [../rest-api/assets/marketplace-apps/create-meeting-sdk.json](../rest-api/assets/marketplace-apps/create-meeting-sdk.json)
- Web: [web/SKILL.md](web/SKILL.md)
- Android: [android/SKILL.md](android/SKILL.md)
- iOS: [ios/SKILL.md](ios/SKILL.md)
- Windows: [windows/SKILL.md](windows/SKILL.md)
- Linux bots: [linux/SKILL.md](linux/SKILL.md)
- Signature playbook: [references/signature-playbook.md](references/signature-playbook.md)
- Troubleshooting: [references/troubleshooting.md](references/troubleshooting.md)
