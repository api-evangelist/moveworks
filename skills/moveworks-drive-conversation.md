---
name: Drive an AI Assistant conversation
description: Create a conversation with the Moveworks AI Assistant, request responses (optionally streaming), read messages, and submit feedback.
api: openapi/moveworks-conversations-api-openapi.yaml
operations: [create-conversation, create-response, create-response-stream, list-conversation-messages, get-response, submit-feedback]
---

# Drive an AI Assistant conversation

Programmatically converse with the Moveworks AI Assistant via the Conversations API (`/assistant/v1`).

## Steps
1. **create-conversation**: `POST /conversations` — returns a `conversation_id`.
2. **create-response**: `POST /conversations/{conversation_id}/responses` with the user's input to get an assistant response (`response_id`). For token-by-token output use **create-response-stream** (`POST /conversations/{conversation_id}/responses/stream`).
3. **get-response**: `GET /conversations/{conversation_id}/responses/{response_id}` to poll a response.
4. **list-conversation-messages**: `GET /conversations/{conversation_id}/messages` to read the transcript (cursor pagination via `pageToken`/`pageSize`).
5. **submit-feedback**: `POST /conversations/{conversation_id}/responses/{response_id}/messages/{message_id}/feedback` to record thumbs up/down.

## Rules
- Auth: `Authorization: Bearer <token>` on every call (HTTP Bearer / OAuth client-credentials token).
- Base host: `https://api.moveworks.ai/assistant/v1` (region-specific host as needed).
- Paginate list endpoints with `pageToken`/`pageSize`; follow `next_page`.
- Error envelope: `{"error":{"code","message"}}`; handle `401` (re-auth), `404`, `429` (back off).
- A `v1beta1` beta variant exists (openapi/moveworks-beta-conversations-api-openapi.yaml) — prefer stable `/assistant/v1`.
