---
name: Notify employees for an event
description: Obtain an OAuth token and send an event-triggered message to a list of employees via the Moveworks Events API.
api: openapi/moveworks-events-api-openapi.yaml
operations: [create-o-auth-token, test-auth, send-message-for-event]
---

# Notify employees for an event

Send a proactive message to employees for an event defined in the Moveworks Events Workspace.

## Auth
1. Get an access token with **create-o-auth-token**: `POST /oauth/v1/token` with body `{client_id, client_secret, grant_type: "client_credentials"}`. The response `data.access_token` is valid ~60 seconds, so fetch one per batch.
2. (Optional) Confirm the credential with **test-auth**: `GET /rest/v1/auth/test` sending `Authorization: Bearer <token>`.

## Send
3. Call **send-message-for-event**: `POST /rest/v1/events/{event_id}/messages/send` with `Authorization: Bearer <token>` and body `{message, recipients[], context}`.
   - `message` is Moveworks-flavored HTML; `recipients` are employee email addresses.
   - The event referenced by `event_id` must already exist in the Events Workspace.

## Rules
- Base host: `https://api.moveworks.ai` (use the correct regional host for EU/CA/Gov/JP/UK).
- On `429` (ErrorRateLimitExceeded) back off and retry; contact support for higher limits.
- No idempotency key is supported — do not blindly retry `send` on a 2xx timeout; re-check before resending.
- Error envelope: `{"error":{"code","message"}}`.
