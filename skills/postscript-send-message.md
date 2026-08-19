---
name: Send a message to a Postscript subscriber
description: >-
  Send a promotional, transactional or conversational SMS/MMS to an existing subscriber and track the
  request through to the sent message.
api: openapi/postscript-messages-api-openapi.yml
operations:
  - create-message
  - get-message-request
  - get-sent-message
  - verify-identity
generated: '2026-08-13'
method: generated
source: >-
  openapi/postscript-messages-api-openapi.yml,
  https://developers.postscript.io/reference/create-message
---

# Send a message to a Postscript subscriber

This spends the merchant's money and puts text on a real person's phone. Treat every call as
irreversible.

## Steps

1. **Confirm the caller.** `verify-identity` (`GET /api/v2/me`).

2. **Send.** `create-message` (`POST /api/v2/message_requests`).
   - `body` is **required**.
   - Address the recipient with **either** `subscriber_id` **or** `phone` — one is required. The
     subscriber must already exist; this operation does not create one.
   - `country` is an ISO Alpha-2 code used to parse `phone` more accurately. Default `US`. Set it
     whenever the number may not be US.
   - `category` is `promotional` (default), `transactional` or `conversational`. Choose truthfully —
     the category governs what the merchant is permitted to send and when.
   - `scheduled_at` is an ISO 8601 datetime for future delivery; omit or `null` to send ASAP.
   - `media_url` turns the send into an **MMS**, with different cost and character limits. Accepted
     types are GIF, PNG and JPEG; size limit 1MB for those, 500KB for other accepted media.

3. **Read the outcome.** A success is `202 Accepted` — the message is queued, not delivered. Poll
   `get-message-request` (`GET /api/v2/message_requests/{id}`) for the request's state, and
   `get-sent-message` (`GET /api/v2/sent_messages/{id}`) for the message Postscript actually sent.

## Rules an agent must follow

- **There is no idempotency key.** Postscript documents none on this operation. A retry after a
  timeout can send the message twice, to a real handset, billed twice. On an ambiguous failure, do
  **not** resend — poll `get-message-request` first, and require human confirmation before a second
  attempt.
- **Never send promotional content as `transactional`.** Category is a compliance declaration, not a
  routing hint. Transactional sending also requires the shop to have it enabled.
- **Respect consent.** Postscript enforces TCPA double opt-in on acquisition; do not attempt to
  message anyone who is not an opted-in subscriber, and stop immediately on an opt-out
  (`shop.subscriber.opt_out`).
- **Cost is per message segment.** SMS is 160 characters (70 with emoji) per segment and each segment
  bills; MMS allows up to 1,600 characters. Adding `media_url` changes the rate. Long bodies are a
  billing decision, not a formatting one.
- **Rate limit is 15 requests/second per token**; `429` returns `error_code: 3002` with no
  `Retry-After`. Back off exponentially with jitter.
- **There is no sandbox and no test number range.** Do not "test" this operation against an arbitrary
  phone number.
