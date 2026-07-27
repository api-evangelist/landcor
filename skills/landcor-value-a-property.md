---
name: Value a BC property with Landcor
description: Resolve a British Columbia street address to a Landcor PID, then pull the property detail, the AVM valuation range and the valuation history for it.
api: openapi/landcor-property-api-openapi.json
operations:
  - autocomplete_address_address_autocomplete_get
  - search_property_property_search_get
  - read_property_property__pid__get
  - read_valuation_range_valuationRange__pid__get
  - read_valuation_history_valuationRange__pid__history_get
generated: '2026-07-26'
method: generated
---

# Value a BC property with Landcor

Landcor keys everything on its own **PID**, a nine-digit identifier written `xxx-xxx-xxx`. You almost never start with one, so every flow begins by resolving an address to a PID.

## Before you start

- Base URL is `https://api.landcor.com`. The harvested OpenAPI declares no `servers[]`, so a generated client will have no base URL — set it yourself.
- Send `Authorization: Bearer <token>` on every call below. Only `GET /health` works without it.
- Without a token you get `HTTP 401 {"detail":"Missing token"}`. There is no signup page and no key-issuance route; Landcor issues tokens through a commercial arrangement (1-866-LANDCOR, https://www.landcor.com/contact/). If you have no token, stop — retrying will not help.
- Coverage is British Columbia residential property only.

## Steps

1. **Resolve the address.** Call `autocomplete_address_address_autocomplete_get` (`GET /address/autocomplete`) with `q` set to the partial address. `q` must be 3–200 characters; `limit` is 1–50. Each word in the query must match a word boundary in the address. Read `suggestions` and `count` from the `AutocompleteResponse`.
2. **Or search by structured filters.** Call `search_property_property_search_get` (`GET /property/search`) with any of `unit_number`, `street_direction`, `street_number`, `street_name`, `postal_code`. All five are optional, so an unfiltered call is legal and will return a large unbounded array — always supply at least `street_name` plus `postal_code` or `street_number`. The response is an array of `PropertySearchResult` (`pid`, `jurisdiction`, `address`, `actual_use_type`); pick the row whose address matches and keep its `pid`.
3. **Pull the property detail.** Call `read_property_property__pid__get` (`GET /property/{pid}`). The `pid` path parameter is validated against `^\d{3}-\d{3}-\d{3}$`; anything else returns `422`. The `PropertyResponse` arrives as a fixed composition of seven sections — `address`, `assessment`, `usage`, `exterior`, `interior`, `other`, `sale`. There is no field expansion or sparse-fieldset parameter; you always receive all of it.
4. **Keep two codes for later.** From the detail response, record `assessment.neighbourhood_code` and `usage.unit_type_code`. The neighbourhood sales series needs both as path parameters and offers no other way to obtain them.
5. **Get the AVM range.** Call `read_valuation_range_valuationRange__pid__get` (`GET /valuationRange/{pid}`) for `low_range_value` and `high_range_value`. Do **not** use `GET /valuationRange/{pid}/updates` — the spec states it now mirrors this endpoint and returns the identical payload.
6. **Get the trend.** Call `read_valuation_history_valuationRange__pid__history_get` (`GET /valuationRange/{pid}/history`) for a `points[]` series of `snapshot_date`, `low_range_value`, `high_range_value`.

## Rules

- **A valid-looking PID is not a PID with data.** Steps 5 and 6 raise `404` when no valuation record or history exists for the PID, per the spec's own business rules. `404` is *not* declared as a response object in the contract, so generated clients will not anticipate it — handle it explicitly.
- **Error envelope.** Application errors are `{"detail": "<string>"}`. Validation failures are `422` with `{"detail": [{loc, msg, type}, ...]}` — read `loc` to find the offending parameter. Nothing here is RFC 9457 problem+json.
- **No pagination.** Search results and history points are unbounded arrays. Narrow the query rather than expecting a cursor.
- **No retries to lean on.** There is no idempotency key and no documented rate limit or `429`. Volume is governed contractually: the Acceptable Use policy forbids stockpiling data pulls and sharing licences between users, so run one call per real question, not speculative sweeps.
- Full conventions: `conventions/landcor-conventions.yml`. Full error catalog: `errors/landcor-problem-types.yml`.
