---
name: Pull Landcor comparables and neighbourhood market context
description: Build a comparables and market picture for a BC property — comparable sales for the subject PID plus the aggregated neighbourhood sales series — and hand the assembled data to Landcor's narrative AVM summary generator.
api: openapi/landcor-property-api-openapi.json
operations:
  - read_property_property__pid__get
  - read_comparables_comparables__pid__get
  - read_neighbourhood_sales_series_valuation_neighbourhood__neighbourhood_code___unit_type_code__sales_get
  - generate_avm_summary_generate_avm_summary_post
generated: '2026-07-26'
method: generated
---

# Pull Landcor comparables and neighbourhood market context

The appraiser / analyst flow. Order matters here because two of the four operations need identifiers that only a prior call produces.

## Before you start

- Base URL `https://api.landcor.com`; `Authorization: Bearer <token>` on every call.
- Start from a resolved Landcor PID (`xxx-xxx-xxx`).

## Steps

1. **Property detail first — it is the key ring.** `read_property_property__pid__get` (`GET /property/{pid}`). Keep three things from the `PropertyResponse`:
   - `assessment.neighbourhood_code`
   - `usage.unit_type_code`
   - the subject's own `exterior`, `interior`, `usage` and `sale` sections, which you will need in step 4.
2. **Comparables.** `read_comparables_comparables__pid__get` (`GET /comparables/{pid}`). `ComparablesResponse.comparables[]` is a list of `ComparableProperty`, each repeating the same seven-section composition as the subject plus `distance_to_subject_property` and `distance_to_subject_property_meters`. The list is unbounded — there is no `limit` and no pagination — so filter client-side on distance and `usage.actual_use_type`.
3. **Neighbourhood market series.** `read_neighbourhood_sales_series_valuation_neighbourhood__neighbourhood_code___unit_type_code__sales_get` (`GET /valuation/neighbourhood/{neighbourhood_code}/{unit_type_code}/sales`) using the two codes from step 1. Query parameters: `interval` (`monthly` or `rolling3m`), `months` (integer ≥ 1), `snapshot_day` (1–31). Response `points[]` carries `period_start`, `period_end`, `min_sale_price`, `max_sale_price`, `avg_sale_price`, `median_sale_price`, `data_source` — read `data_source` before quoting a figure. Use `rolling3m` for thin neighbourhoods where monthly buckets are noisy.
4. **Narrate it, if you want the summary.** `generate_avm_summary_generate_avm_summary_post` (`POST /generate-avm-summary`) takes a `LandcorAVMSummaryRequest` you assemble yourself: `subject_property`, `valuation`, `assessment_history[]`, `comparable_sales[]`, `neighbourhood`, `sales_history[]`, `valuation_change`, `climate_events`, `permit_history`, `scores`. It returns `summary` plus a `validation` object (`valid`, `messages`). Check `validation` before using `summary`.

## Rules

- **The summary input does not reuse the response schemas.** `SubjectProperty`, `ComparableSale` and `ValuationMeta` are a parallel vocabulary for the same facts, with different field names — `SubjectProperty.floor_area_sqft` is not `InteriorDataSection.total_finished_area`, and `ComparableSale` is flat where `ComparableProperty` is sectioned. You must map by hand. The mapping is not published; see `data-model/landcor-data-model.yml` for the shape of both sides.
- **Nothing is invented for you.** `generate-avm-summary` narrates only what you send. If you omit `climate_events` or `permit_history`, the summary simply will not cover them — do not fill those blocks with data from anywhere other than Landcor's own responses.
- **Errors.** `422` for validation with `{"detail": [{loc, msg, type}]}`; `404` (undeclared) when the PID has no record; `401 {"detail":"Missing token"}` without a bearer token. See `errors/landcor-problem-types.yml`.
- **No events, no callbacks.** Landcor publishes no webhooks and no AsyncAPI. Neighbourhood series must be polled; there is no subscription to value changes.
