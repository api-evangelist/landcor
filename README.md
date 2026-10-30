# Landcor Data

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

Landcor Data Corporation is a New Westminster, British Columbia property data and automated valuation company, founded in 2000, that sells residential valuations, assessment detail and land title documents across the roughly 1.9 million residential properties in BC. It sits in the valuation and public-record layer of the Canadian value chain rather than the listings layer: its inputs are BC Assessment detail, Land Title and Survey Authority (LTSA) title and document search, BC Registry Services, municipal tax certificates and materials licensed from the Integrated Cadastral Information Society (ICIS), and its outputs are the Valuator AVM report, the Adjusted Value Profiler, the Property Profiler, Title Search Plus and historic valuation reports sold to lenders, appraisers, notaries, insurers and brokerages. Its API posture is the unusual case in this study: a real, live, anonymously readable machine-readable contract exists with no developer programme around it. The host api.landcor.com runs a FastAPI service on Azure App Service in Canada Central that serves a valid OpenAPI 3.1.0 document titled "Landcor Property API" version 0.1.0 at /openapi.json, with Swagger UI at /docs and ReDoc at /redoc, all returning HTTP 200 without credentials. Twelve operations cover property search, property detail, PDF report retrieval, valuation range, valuation history, loan-to-value checks, neighbourhood sales series, comparables, address autocomplete and an AVM narrative summary. Every operation except /health requires an HTTP Bearer token and returns 401 "Missing token" without one, and no route to obtain that token is published anywhere: landcor.com carries no developer, API, partner or data-licensing page, does not link api.landcor.com at all, and the store.landcor.com self-serve account is for buying individual reports through the web store, not for API credentials. RESO is absent, which is the expected Canadian answer. Landcor is not among the nineteen Canadian organizations RESO lists as members, holds no Web API or Data Dictionary certification, exposes no OData $metadata document, and uses its own PID identifier rather than the RESO Universal Property Identifier. No open, unlicensed dataset is published: the underlying assessment, title and cadastral data is licensed to Landcor from provincial bodies and resold, so the public record itself is a commercial product here.

**Areas:** Real Estate · Canada · Valuation · AVM · Property Records · Title · Land Registry · Mortgage · PropTech · Property Data

## Landcor Data on APIs.io

Everything profiled for Landcor Data is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/landcor/](https://apis.io/providers/landcor/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [Landcor Data AVM Summary API](https://apis.io/apis/landcor/landcor-avm-summary-api/) — The AVM Summary API from Landcor Data — 1 operation(s) for avm summary.
- [Landcor Data Comparables API](https://apis.io/apis/landcor/landcor-comparables-api/) — The Comparables API from Landcor Data — 1 operation(s) for comparables.
- [Landcor Data Health API](https://apis.io/apis/landcor/landcor-health-api/) — The Health API from Landcor Data — 1 operation(s) for health.
- [Landcor Data Property API](https://apis.io/apis/landcor/landcor-property-api/) — The Property API from Landcor Data — 4 operation(s) for property.
- [Landcor Data Valuation API](https://apis.io/apis/landcor/landcor-valuation-api/) — The Valuation API from Landcor Data — 5 operation(s) for valuation.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (2)

Multi-step workflows that chain operations into a business task.

- [Landcor — address to valuation](https://apis.io/arazzos/landcor/landcor-address-to-valuation/)
- [Landcor — loan-to-value check and valuation report](https://apis.io/arazzos/landcor/landcor-ltv-check-and-report/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [landcor](https://apis.io/servers/landcor/landcor/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Landcor Agentic Access](https://apis.io/agentic-access/landcor/landcor-agentic-access/)

#### Collections (1)

Postman and Open Collection request collections.

- [Landcor Property API](https://apis.io/collections/landcor/open-landcor-property-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Landcor Property Api Overlay](https://apis.io/overlays/landcor/landcor-property-api-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Landcor Authentication](https://apis.io/security/landcor/landcor-authentication/)
- [Landcor Domain Security](https://apis.io/security/landcor/landcor-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.landcor.com/)
- [Documentation](https://api.landcor.com/docs)
- [API reference](https://api.landcor.com/redoc)
- [Pricing](https://www.landcor.com/pricing/)
- [Sign up](https://store.landcor.com/user/user_add.aspx)
- [Support](https://www.landcor.com/support/)
- [Blog](https://www.landcor.com/about-us/landcor-news/)
- [Terms of service](https://www.landcor.com/acceptable-use/)
- [Privacy policy](https://www.landcor.com/privacy-policy/)
- [GitHub](https://github.com/Landcor)

## API Evangelist rating

**Kin Score 37.3** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 45.0 |
| Contract governance | 4.5 |
| Operational transparency | 13.2 |
| Developer ergonomics | 37.5 |
| Access clarity | 44.7 |

**Agent readiness 26.4** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/landcor/](https://apis.io/providers/landcor/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Landcor Data: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/landcor/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
