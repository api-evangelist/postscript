---
name: Send a custom event into Postscript Flows
description: >-
  Push a behavioural or transactional event from your system into a Postscript merchant's account so
  it can trigger an SMS Flow, using the Custom Events API.
api: openapi/postscript-events-api-openapi.yml
operations:
  - create-custom-event
  - verify-identity
generated: '2026-08-13'
method: generated
source: >-
  openapi/postscript-events-api-openapi.yml,
  https://developers.postscript.io/reference/create-custom-event
---

# Send a custom event into Postscript Flows

Use this when your system observes something a merchant wants to text about — an order shipped, a
subscription renewed, a product came back in stock — and Postscript should react to it.

## Before you start

- Base URL is `https://api.postscript.io`. Everything is under `/api/v2/`.
- Authenticate with a **private** API key: `Authorization: Bearer sk_...`. Never call this from a
  browser; Postscript's own docs tell you to proxy through your backend.
- If you are a **partner** acting for a shop, send your partner key in `Authorization` **and** that
  shop's private key in `X-Postscript-Shop-Token`. Getting this backwards produces confusing
  plan-limitation errors, not auth errors.
- There is no sandbox. Every call is live against a real merchant.

## Steps

1. **Confirm who you are.** Call `verify-identity` (`GET /api/v2/me`) once at startup to check the
   token resolves to the shop or partner you expect. Do this before any write.

2. **Send the event.** Call `create-custom-event` (`POST /api/v2/events`).
   - `type` is required. Allowed characters are `a-z`, `A-Z`, `0-9` and `_`. Postscript prefixes your
     partner or shop name automatically, so `OrderPlaced` shows in the merchant UI as
     `[Your Name] - OrderPlaced`. Do not add your own prefix.
   - Identify the subscriber with **one** of `subscriber_id`, `phone` or `email`. Prefer
     `subscriber_id` (`s_` prefixed) when you have it; use E.164 for `phone`.
   - `occurred_at` is optional; omit it and Postscript stamps receipt time. Format is
     `%Y-%m-%d %H:%M:%S.%f` in UTC.
   - `properties` is a free-form key:value object. Postscript infers the type (string, boolean,
     integer, float, datetime string) and exposes each key for segmentation and merge tags. Spaces in
     key names are allowed.
   - `external_id` is your own id for the event. Set it — it is the only correlation handle you get.

3. **Handle the response.** A success is `202 Accepted`: the event is queued, not processed. There is
   no read-back operation for events, so a 202 is the last thing you will ever see about it. A `400`
   returns `{"error_code": ..., "error_message": ..., "success": false}`.

## Rules an agent must follow

- **Do not retry blindly.** `external_id` is documented as *your* unique id, **not** as an
  idempotency key — Postscript does not state that a repeated `external_id` is de-duplicated. A retry
  may fire the merchant's Flow twice and send a second text. Retry only on a transport failure or a
  429, never on a 4xx.
- **Rate limit is 15 requests/second per token** (per partner, per shop, or per partner+shop pair).
  Exhaustion returns `429` with `error_code: 3002`. No `Retry-After` or `RateLimit-*` header is
  returned, so back off exponentially with jitter on your own schedule.
- **The event name is a contract with the merchant.** A merchant builds a Flow against the name that
  appears in their UI. Changing `type` silently breaks their automation — treat it as a published
  interface.
- Never send a customer's data to an event `type` the merchant has not agreed to; the properties you
  attach become merge tags in messages sent to real handsets.
