# Landcor Data (landcor)

Landcor Data Corporation is a New Westminster, British Columbia property data and automated valuation company, founded in 2000, that sells residential valuations, assessment detail and land title documents across the roughly 1.9 million residential properties in BC. It sits in the valuation and public-record layer of the Canadian value chain rather than the listings layer. Its inputs are BC Assessment detail, Land Title and Survey Authority (LTSA) title and document search, BC Registry Services, municipal tax certificates and materials licensed from the Integrated Cadastral Information Society (ICIS), and its outputs are the Valuator AVM report, the Adjusted Value Profiler, the Property Profiler, Title Search Plus and historic valuation reports sold to lenders, appraisers, notaries, insurers and brokerages. Its API posture is the unusual case in this study: a real, live, anonymously readable machine-readable contract exists with no developer programme around it. The host api.landcor.com runs a FastAPI service on Azure App Service in Canada Central that serves a valid OpenAPI 3.1.0 document titled "Landcor Property API" version 0.1.0 at /openapi.json, with Swagger UI at /docs and ReDoc at /redoc, all returning HTTP 200 without credentials. Twelve operations cover property search, property detail, PDF report retrieval, valuation range, valuation history, loan-to-value checks, neighbourhood sales series, comparables, address autocomplete and an AVM narrative summary. Every operation except /health requires an HTTP Bearer token and returns 401 "Missing token" without one, and no route to obtain that token is published anywhere. RESO is absent, which is the expected Canadian answer, and no open, unlicensed dataset is published.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/landcor/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/landcor/refs/heads/main/apis.yml)

## Tags

- Real Estate
- Canada
- Valuation
- AVM
- Property Records
- Title
- Land Registry
- Mortgage
- PropTech
- Property Data

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### Landcor Property API

A live REST service on `api.landcor.com` that publishes a valid OpenAPI 3.1.0 contract, served anonymously at `https://api.landcor.com/openapi.json` (HTTP 200, 47,840 bytes, fetched 2026-07-26) with interactive Swagger UI at `/docs` and ReDoc at `/redoc`. Twelve operations across five tags: Property (`GET /property/search`, `GET /property/{pid}`, `GET /property/{pid}/report/pdf`, `GET /address/autocomplete`), Valuation (`GET /valuationRange/{pid}`, `GET /valuationRange/{pid}/updates`, `GET /valuationRange/{pid}/history`, `POST /valuation/ltv-check`, `GET /valuation/neighbourhood/{neighbourhood_code}/{unit_type_code}/sales`), Comparables (`GET /comparables/{pid}`), AVM Summary (`POST /generate-avm-summary`) and Health (`GET /health`). Properties are keyed on the Landcor PID in `xxx-xxx-xxx` format.

- **Human URL:** [https://api.landcor.com/docs](https://api.landcor.com/docs)
- **Base URL:** `https://api.landcor.com`

#### Tags

- Property
- Valuation
- AVM
- Comparables
- Real Estate
- Canada

#### Properties

- [OpenAPI](openapi/landcor-property-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://api.landcor.com/docs)
- [API Reference](https://api.landcor.com/redoc)

## RESO Posture

**Not RESO-certified. No RESO reference found.**

RESO's own [Canadian Membership](https://www.reso.org/canadian-membership/) page lists nineteen Canadian member organizations, including CREA, Centris, MPAC, the Real Estate Board of Greater Vancouver and the REALTORS Association of Edmonton, and Landcor is not among them. No RESO Web API certification, no Data Dictionary certification or version, no certification directory entry, no OData `$metadata` document (`https://api.landcor.com/$metadata` and `/odata/$metadata` both return HTTP 404) and no Universal Property Identifier (UPI) usage was observed. Landcor keys everything on its own PID in `xxx-xxx-xxx` format. The absence is structurally expected twice over: RESO certification is a US industry mandate driven by NAR and applies to MLSs, and Landcor is not a listings company at all but a valuation and public-record reseller whose sources are BC Assessment, the LTSA and BC Registry Services, none of which RESO governs.

## Access Gate

**`none-published`.** Certification is not reachability, and here neither is documentation. The contract is completely open; the credentials are completely closed. Every operation except `GET /health` declares an `HTTPBearer` scheme and returns HTTP 401 `{"detail":"Missing token"}` when called anonymously, verified against `/property/search`, `/address/autocomplete` and `/valuationRange/{pid}` on 2026-07-26. Yet there is no developer portal on landcor.com, no API programme page, no key request form, no partner page, no data-licensing page, no API pricing and no API terms, and the marketing site never references `api.landcor.com` at all. `developer.`, `developers.`, `docs.`, `valuator.`, `portal.`, `ws.` and `data.landcor.com` do not resolve in DNS. What Landcor does publish is a self-serve retail signup at [store.landcor.com](https://store.landcor.com/user/user_add.aspx) that buys individual reports through the web store; it is not an API credential. The binding document a customer accepts is the [Acceptable Use](https://www.landcor.com/acceptable-use/) policy, which obliges users "not to stockpile data pulls" and "to not share licenses among offices or to share data pulls among various users" — per-seat, no-stockpiling terms that are the plainest statement of why bulk API access is not offered self-serve.

## Open Data

**None.** Every data product is resold from licensed provincial sources: BC Assessment property detail, LTSA title documents and plans, BC Registry Services, municipal tax certificates and ICIS cadastral materials. The Acceptable Use policy does reference the Open Government Licence — British Columbia, but that is Landcor consuming provincial open data as an input, not publishing an open output. Canada has no counterpart to HM Land Registry Price Paid or Ordnance Survey open products, and land registration is provincial and largely privatised, so the public record reaches the market as a priced commercial product.

## Auth Model

**HTTP Bearer token, issued out of band.** The harvested OpenAPI declares exactly one security scheme, `components.securitySchemes.HTTPBearer` with `type: http` and `scheme: bearer`. There is no OAuth 2.0 authorization or token endpoint, no client-credentials flow, no scopes, no API-key header and no refresh mechanism. No OpenID Connect discovery document is served: `/.well-known/openid-configuration` returns HTTP 404 on `api.landcor.com`, `www.landcor.com` and `store.landcor.com` alike. No SAML member portal was found. The retail store uses ordinary ASP.NET forms sign-in with a secret question — a customer login, not a developer identity system.

## Webhooks, Events, SDKs, Postman

None found, and the absence is the finding. The OpenAPI contains no `webhooks` object, no callbacks and no event operations; all twelve operations are synchronous request/response. No SDK, client library or CLI is published. The [GitHub organization](https://github.com/Landcor) is real (type Organization, name "Landcor Data Corporation", created 2026-06-05) but holds one public repository, `.github`, with no API artifacts. No Postman workspace or collection, no changelog, no status page and no versioning policy; the contract self-reports version `0.1.0`.

## Notes

The specification's operation descriptions are unprocessed Python docstrings that disclose implementation detail, naming the stored procedure `USP_SEARCH_SERVICE_PROPERTY` behind `/property/search` and "the legacy Landcor SOAP webservice" that produces the base64-encoded, password-protected PDF behind `/property/{pid}/report/pdf`. That independently corroborates Landcor's own [Full Stack .NET Developer](https://www.landcor.com/full-stack-net-developer/) posting, which asks the hire to "Build and enhance APIs (REST and SOAP) that integrate with internal systems." The public REST contract is a modern facade over an older SOAP estate. The stock WordPress REST API at `www.landcor.com/wp-json/` is reachable but is a CMS artifact of the marketing site, not a Landcor product, and is deliberately not listed.

## Common Properties

- [Website](https://www.landcor.com/)
- [Products](https://www.landcor.com/online-property-tools/)
- [Pricing](https://www.landcor.com/pricing/)
- [Sign Up](https://store.landcor.com/user/user_add.aspx)
- [Login](https://store.landcor.com/user/login.aspx)
- [Support](https://www.landcor.com/support/)
- [Contact](https://www.landcor.com/contact/)
- [Blog](https://www.landcor.com/about-us/landcor-news/)
- [Blog RSS](https://www.landcor.com/feed/)
- [Privacy Policy](https://www.landcor.com/privacy-policy/)
- [Terms of Service](https://www.landcor.com/acceptable-use/)
- [Security Policy](https://www.landcor.com/security-policy/)
- [GitHub Organization](https://github.com/Landcor)
- [LinkedIn](https://ca.linkedin.com/company/landcor-data-corporation)

## Maintainers

- **Kin Lane** — kin@apievangelist.com
