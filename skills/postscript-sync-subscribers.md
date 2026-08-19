---
name: Sync and segment Postscript subscribers
description: >-
  Page, filter and sort a shop's SMS subscriber list, read an individual subscriber, and update
  subscriber fields and custom properties.
api: openapi/postscript-subscribers-api-openapi.yml
operations:
  - get-subscribers
  - get-subscriber
  - update-subscriber-by-id
  - get-keywords
  - get-keyword
  - verify-identity
generated: '2026-08-13'
method: generated
source: >-
  openapi/postscript-subscribers-api-openapi.yml,
  openapi/postscript-keywords-api-openapi.yml,
  https://developers.postscript.io/reference/get-subscribers
---

# Sync and segment Postscript subscribers

Use this to mirror a merchant's SMS list into your own system, enrich subscribers with properties
from your data, or pull a segment for reporting.

## Steps

1. **Resolve the caller.** `verify-identity` (`GET /api/v2/me`) tells you which shop or partner the
   token acts as. Log it — a partner integration serving many shops must never mix them up.

2. **Page the list.** `get-subscribers` (`GET /api/v2/subscribers`) takes a `page` query parameter.
   There is no page-size parameter and no cursor or `next` link, so iterate `page=1,2,3…` until a
   page comes back empty.

3. **Filter server-side, not client-side.** Every filterable field takes suffixed operators:
   - `__eq`, `__gt`, `__gte`, `__lt`, `__lte` on `created_at` and `updated_at`
   - `__eq`, `__contains`, `__in` on `email`, `phone_number`, `shopify_customer_id`
   - `ps_id__eq` for the Postscript tracking id

   An incremental sync is `updated_at__gte=<last successful sync timestamp>` — do that rather than
   walking the whole list every run.

4. **Sort deterministically.** `sort` takes `{field}__asc` or `{field}__desc`. Pin a sort when paging,
   or concurrent writes will shuffle rows between pages and you will miss records.

5. **Read one subscriber.** `get-subscriber` (`GET /api/v2/subscribers/{id}`) with the `s_`-prefixed
   id.

6. **Update.** `update-subscriber-by-id` (`PATCH /api/v2/subscribers/{id}`) writes contact fields and
   the free-form `properties` object. Property values may be strings, booleans, integers, floats or
   datetime strings; spaces in property names are allowed.

7. **Attribute the source.** `get-keywords` / `get-keyword` read the shop's opt-in keywords so you can
   report which growth source a subscriber came from. Keywords are read-only over the API — they are
   created in the Postscript dashboard.

## Rules an agent must follow

- **Use prefixed ids.** Subscribers are `s_…`, shops are `shop_…`. Legacy unprefixed v1 ids are still
  accepted "for a period of time" with no published sunset — migrate off them; they were tied to a
  shop's API keys and broke on rotation.
- **PATCH is not a merge you can assume.** Send only the fields you intend to change, and re-read the
  subscriber to confirm rather than trusting your local copy.
- **Never write an opt-in state.** Opt-in and opt-out are consent events, not fields to set. Use the
  compliance operations (see `postscript-unsubscribe-and-redact`), and let Postscript's TCPA double
  opt-in do its job.
- **Rate limit is 15 requests/second per token.** A full-list sync will hit it. Throttle to well under
  15/s and back off exponentially with jitter on `429` (`error_code: 3002`) — no `Retry-After` header
  is returned.
- **There is no sandbox.** Every read is real customer PII and every write lands on a live list.
  Postscript publishes no subscriber-create endpoint in its API reference, so do not attempt to
  invent one.
