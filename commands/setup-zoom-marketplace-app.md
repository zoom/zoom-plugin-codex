---
description: Select, create, or validate the Zoom Marketplace app, manifest, scopes, events, and credential model required by an integration.
---

# Setup Zoom Marketplace App

Use this command before implementation when a Zoom app must be selected, created, validated, or corrected.

## Preflight

1. Inspect the repository for Zoom products, auth code, callback URLs, event receivers, SDK signature helpers, env templates, and existing manifest files.
2. Identify the target scenario, actor, account ownership, and whether an existing Marketplace app should be reused or changed.
3. Classify the app model: user-managed General, admin-managed General, S2S OAuth, Meeting SDK, or Build Platform.
4. Do not print existing client secrets, access tokens, refresh tokens, SDK secrets, or webhook secret tokens.

## Plan

Before editing:

- state the selected app model and why it fits
- name the template or existing manifest being used
- list required OAuth scopes, app-owned scopes, features, events, callback URLs, and domains
- state which setup must still be completed in Marketplace after API creation

## Commands

1. Select the narrowest scenario template from `skills/rest-api/references/marketplace-app-templates.md`.
2. Replace all sample names, URLs, domains, contacts, command names, and account-specific values.
3. Reduce scopes to those required by the exact target operations; do not combine user and admin scope classes.
4. For General Apps, validate the inner manifest and treat HTTP `200` with `ok: false` as a validation failure.
5. Create General Apps through `/v2/marketplace/apps`; use `/v2/accounts/{accountId}/marketplace/apps` for S2S and Meeting SDK app creation when authorized.
6. Complete post-create WebSocket delivery, webhook, RTMS, or product feature setup that the public schema does not reliably encode.
7. Put generated secrets in the repo's existing secret-management path or report the required secret names without exposing values.

## Verification

1. Validate every edited JSON file with a JSON parser.
2. Re-run General App manifest validation after customization.
3. Confirm app type, usage, scope suffixes, callback URLs, domain objects, products, and feature flags match the intended workflow.
4. Fetch or export the created app when possible and compare normalized configuration with the intended template.
5. Report licensing, entitlement, authorization, review, or post-create UI steps that remain.

## Summary

```text
## Result
- Action: selected, created, or validated a Zoom Marketplace app
- Status: success | partial | failed
- Details: app model, template, scopes, features/events, validation, remaining setup
```

## Next Steps

- Continue with the owning product build command.
- Run `/setup-zoom-oauth` after the app model and scopes are stable.
