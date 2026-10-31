# Postscript

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Postscript is an SMS and RCS marketing platform built for Shopify brands, covering list growth, campaign and automation delivery, conversational messaging, AI shopping assistants and TCPA compliance. Its developer surface is the Partner API v2 at api.postscript.io, a twenty-operation REST API authenticated with private API keys, through which partners and enterprise shops read and update SMS subscribers, read opt-in keywords, push custom events into Flow Builder, send promotional, transactional and conversational messages, manage webhook subscriptions for shop and subscriber lifecycle events, and run compliance unsubscribe and redaction operations. A browser JavaScript SDK carries the onsite opt-in popups, the checkout consent checkbox and browse-behaviour event tracking.

**Areas:** SMS · Marketing · Messaging · E-Commerce · Shopify · RCS · Subscribers · Webhook · Compliance

## Postscript on APIs.io

Everything profiled for Postscript is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/postscript/](https://apis.io/providers/postscript/)** — or go straight to one of the APIs or properties below.

### APIs (7)

- [Postscript Subscribers API](https://apis.io/apis/postscript/postscript-subscribers-api/) — Read, filter, page and update a shop's SMS subscribers. Filters use suffixed operators (__eq, __gt, __contains, __in) across created_at, updated_at, email,…
- [Postscript Events API](https://apis.io/apis/postscript/postscript-events-api/) — Send custom events for use in Postscript Flows and triggers.
- [Postscript Keywords API](https://apis.io/apis/postscript/postscript-keywords-api/) — Read the opt-in keywords a shop has configured for subscriber acquisition and attribution.
- [Postscript Messages API](https://apis.io/apis/postscript/postscript-messages-api/) — Send promotional, transactional or conversational SMS and MMS to an existing subscriber, and read the resulting message request and sent message.
- [Postscript Webhooks API](https://apis.io/apis/postscript/postscript-webhooks-api/) — Create, read, update and delete webhook subscriptions, fetch the Postscript-Signature signing token, retrieve example event payloads and trigger test…
- [Postscript Compliance API](https://apis.io/apis/postscript/postscript-compliance-api/) — TCPA opt-out and data redaction for a subscriber, addressable by subscriber id, phone, email or Shopify customer id.
- [Postscript Identity API](https://apis.io/apis/postscript/postscript-identity-api/) — Verify which partner or shop the calling API token resolves to.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Postscript Webhooks](https://apis.io/asyncapis/postscript/postscript-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [postscript-docs](https://apis.io/servers/postscript/postscript-docs/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Postscript Agentic Access](https://apis.io/agentic-access/postscript/postscript-agentic-access/)

#### Overlays (7)

OpenAPI overlays, including conversational phrasing for each operation.

- [Postscript Compliance Api Overlay](https://apis.io/overlays/postscript/postscript-compliance-api-overlay/)
- [Postscript Events Api Overlay](https://apis.io/overlays/postscript/postscript-events-api-overlay/)
- [Postscript Identity Api Overlay](https://apis.io/overlays/postscript/postscript-identity-api-overlay/)
- [Postscript Keywords Api Overlay](https://apis.io/overlays/postscript/postscript-keywords-api-overlay/)
- [Postscript Messages Api Overlay](https://apis.io/overlays/postscript/postscript-messages-api-overlay/)
- [Postscript Subscribers Api Overlay](https://apis.io/overlays/postscript/postscript-subscribers-api-overlay/)
- [Postscript Webhooks Api Overlay](https://apis.io/overlays/postscript/postscript-webhooks-api-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Postscript Authentication](https://apis.io/security/postscript/postscript-authentication/)
- [Postscript Domain Security](https://apis.io/security/postscript/postscript-domain-security/)
- [Postscript Trust Center](https://apis.io/security/postscript/postscript-trust-center/)
- [Postscript Vulnerability Disclosure](https://apis.io/security/postscript/postscript-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Postscript Plans Pricing](https://apis.io/plans/postscript/postscript-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Postscript Rate Limits](https://apis.io/rate-limits/postscript/postscript-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Postscript Finops](https://apis.io/finops/postscript/postscript-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://postscript.io)
- [Documentation](https://developers.postscript.io)
- [Getting started](https://developers.postscript.io/docs/getting-started)
- [API reference](https://developers.postscript.io/reference)
- [Pricing](https://postscript.io/pricing)
- [Sign up](https://postscript.io/partners-signup)
- [Support](https://help.postscript.io)
- [Changelog](https://developers.postscript.io/changelog)
- [Blog](https://postscript.io/blog)
- [Security](https://postscript.io/security)
- [Compliance](https://developers.postscript.io/docs/compliance)
- [Terms of service](https://postscript.io/terms-of-service)
- [Privacy policy](https://postscript.io/privacy)
- [Status](https://status.postscript.io)

## API Evangelist rating

**Kin Score 60.9** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 62.2 |
| Contract governance | 4.5 |
| Operational transparency | 63.2 |
| Developer ergonomics | 41.1 |
| Access clarity | 88.2 |

**Agent readiness 42.8** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 27.9** — Telecommunications

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/postscript/](https://apis.io/providers/postscript/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Postscript: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/postscript/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
