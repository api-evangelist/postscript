---
name: Subscribe to and verify Postscript webhooks
description: >-
  Register an endpoint for Postscript shop events, verify the Postscript-Signature header, and handle
  the at-least-once retry behaviour correctly.
api: openapi/postscript-webhooks-api-openapi.yml
operations:
  - create-webhook-subscription
  - list-webhook-subscriptions
  - get-webhook-subscription
  - update-webhook-subscription
  - delete-webhook-subscription
  - get-webhook-signing-token
  - example-event
  - test-shop-webhook
generated: '2026-08-13'
method: generated
source: >-
  openapi/postscript-webhooks-api-openapi.yml,
  https://developers.postscript.io/docs/configuring-webhooks,
  https://developers.postscript.io/reference/webhook-events
---

# Subscribe to and verify Postscript webhooks

Postscript pushes shop events to an HTTPS endpoint you own. One subscription binds **one** event type
to **one** callback URL, so subscribing to four events means four subscriptions.

## Event types

| Event | Fires when |
|---|---|
| `shop.test` | Synthetic — use it to prove your endpoint works |
| `shop.incoming_message` | A subscriber texts the shop |
| `shop.subscriber.opt_in` | A subscriber opts in to promotional messages |
| `shop.subscriber.opt_out` | A subscriber opts out |
| `shop.email_collected` | An email is captured by a Postscript popup |

Postscript states more types may be added at any time — do not assume the list is closed, and ignore
event names you do not recognise rather than erroring.

> The enum on `create-webhook-subscription` spells the email event `shop.shop.email_collected` while
> the event catalog and `example-event` spell it `shop.email_collected`. If one is rejected, try the
> other. This is Postscript's inconsistency, not a typo here.

## Steps

1. **Build the endpoint first.** Fetch a real payload shape with `example-event`
   (`GET /api/v2/webhooks/example?event=shop.incoming_message`) and code against it. Every delivery
   has the same envelope: `webhook_id`, `resource_type`, `resource_id`, `event_time`, `event`,
   `event_data`.

2. **Fetch and store the signing token.** Call `get-webhook-signing-token`
   (`GET /api/v2/webhooks/token`) once and keep the value in your secret store.

3. **Register the subscription.** Call `create-webhook-subscription` (`POST /api/v2/webhooks`) with
   `callback_url` (HTTPS only) and `event`. Optionally pass `headers` — a key:value object Postscript
   will send on every callback, which is the right place for your own routing or auth header.

4. **Verify every delivery.** Compare the `Postscript-Signature` request header to the stored signing
   token. Missing or mismatched ⇒ reject without processing.

5. **Reply `200` fast.** Postscript ignores the body. Acknowledge *before* doing application work.

6. **Test it.** Call `test-shop-webhook` (`POST /api/v2/webhooks/test`), which returns `202`, or text
   the shop's number and wait for `shop.incoming_message`.

7. **Manage subscriptions.** `list-webhook-subscriptions`, `get-webhook-subscription`,
   `update-webhook-subscription` and `delete-webhook-subscription` cover the rest of the lifecycle.

## Rules an agent must follow

- **Deliveries repeat.** If Postscript does not get a `200`, it retries every 5 minutes for up to 1
  hour. The same event with the same data can arrive more than once for other reasons too. Your
  handler must be idempotent — key on `event_data.id` where present, not on `webhook_id`, which
  identifies the *subscription*, not the delivery.
- **Deliveries can be lost.** After 1 hour Postscript stops retrying and the event is gone. If the
  event matters, reconcile — for subscriber state, re-read with `get-subscriber`.
- **The signature is weak.** It is a static shared token echoed in a header, not an HMAC over the
  body and a timestamp. It proves the sender knows the token; it does **not** prove the payload is
  untampered and gives no replay protection. Treat webhook contents as untrusted input, and never let
  a webhook alone authorise a spend, a send, or a data deletion.
- Rotate your handling if the token is ever exposed — re-fetch it from
  `get-webhook-signing-token` and compare against the new value.
