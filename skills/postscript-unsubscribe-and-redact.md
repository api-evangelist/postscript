---
name: Honour an opt-out or erasure request in Postscript
description: >-
  Run the TCPA opt-out and the GDPR/CCPA-style data redaction operations against a Postscript
  subscriber, identified by subscriber id, phone, email or Shopify customer id.
api: openapi/postscript-compliance-api-openapi.yml
operations:
  - unsubscribe
  - redact
  - get-subscriber
  - verify-identity
generated: '2026-08-13'
method: generated
source: >-
  openapi/postscript-compliance-api-openapi.yml,
  https://developers.postscript.io/reference/unsubscribe,
  https://developers.postscript.io/reference/redact,
  https://developers.postscript.io/docs/compliance
---

# Honour an opt-out or erasure request in Postscript

Two distinct operations sit behind `/api/v2/compliance/`. They are not interchangeable.

| Operation | Path | Effect |
|---|---|---|
| `unsubscribe` | `PATCH /api/v2/compliance/unsubscribe` | Opts the subscriber out of Postscript messaging. The record remains. |
| `redact` | `PATCH /api/v2/compliance/redact` | Redacts the subscriber's personal data **and**, per Postscript's own note, also unsubscribes them if they still have an active subscription. |

## Steps

1. **Confirm the caller and the shop.** `verify-identity` (`GET /api/v2/me`). A partner must send its
   own key in `Authorization` and the shop's key in `X-Postscript-Shop-Token`.

2. **Resolve the person.** Both operations accept an email, a phone number, a Shopify customer id or a
   Postscript subscriber id. Prefer the `s_`-prefixed subscriber id when you have it — matching on a
   phone or email is the failure mode that erases the wrong person. Where possible read the record
   first with `get-subscriber` and confirm it is who the request is about.

3. **Choose the right operation.**
   - A "stop texting me" / STOP / opt-out request ⇒ `unsubscribe`.
   - A right-to-erasure / delete-my-data request ⇒ `redact`.

4. **Send it.** Both return `202 Accepted` — queued, not completed. `403 Forbidden` is the documented
   failure.

5. **Record what you did.** Keep the request, the identifier used, the timestamp and the 202. There is
   no read-back operation for either action, so your own log is the only audit trail.

## Rules an agent must follow

- **`redact` is irreversible and there is no sandbox to rehearse it in.** Never run it speculatively,
  never run it in a batch you have not reviewed, and never run it because a heuristic matched a
  phrase. It requires an explicit, verified erasure request from the data subject or the merchant.
- **`redact` is not a bigger `unsubscribe`.** Reaching for it on a plain opt-out destroys data the
  merchant is entitled to keep and the subscriber did not ask you to delete.
- **Do not batch by loose identifier.** `email__contains`-style matching belongs in reporting, never
  in an erasure workflow.
- **Opt-out is time-critical.** Postscript's compliance posture is built on TCPA; honour opt-outs
  immediately and stop every queued send to that subscriber.
- **Never re-add a redacted or unsubscribed person.** Postscript requires a fresh double opt-in — a
  subscriber who does not reply to the confirmation text is not added at all.
- Rate limit is 15 requests/second per token; `429` returns `error_code: 3002`. Back off with jitter.
