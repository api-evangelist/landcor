---
name: Run a Landcor loan-to-value check
description: Test a proposed mortgage amount against Landcor's AVM value for a BC property, and pull the password-protected PDF valuation report for the file.
api: openapi/landcor-property-api-openapi.json
operations:
  - search_property_property_search_get
  - read_valuation_range_valuationRange__pid__get
  - run_ltv_check_valuation_ltv_check_post
  - read_property_pdf_property__pid__report_pdf_get
generated: '2026-07-26'
method: generated
---

# Run a Landcor loan-to-value check

This is the lender / mortgage-broker flow: does the AVM value support the loan being written, and what document goes in the file?

## Before you start

- Base URL `https://api.landcor.com`; `Authorization: Bearer <token>` on every call. No token, no access — see `authentication/landcor-authentication.yml`.
- You need the Landcor **PID** (`xxx-xxx-xxx`). Resolve it first with `search_property_property_search_get` or `autocomplete_address_address_autocomplete_get` (see the *Value a BC property* skill).

## Steps

1. **Confirm the property exists.** `search_property_property_search_get` (`GET /property/search`) with `street_number`, `street_name` and `postal_code`. Take the `pid` from the matching `PropertySearchResult`.
2. **Read the AVM band first.** `read_valuation_range_valuationRange__pid__get` (`GET /valuationRange/{pid}`) returns `low_range_value` and `high_range_value`. Do this before the LTV check so you can report the band, not just a boolean — the check itself does not return the valuation.
3. **Run the check.** `run_ltv_check_valuation_ltv_check_post` (`POST /valuation/ltv-check`) with an `LTVCheckRequest` body: `{"pid": "...", "ltv_amount": <number>}`. The `LTVCheckResponse` returns `pid`, `ltv_amount`, `has_avm` and `avm_exceeds_ltv`.
4. **Interpret honestly.** `has_avm: false` means Landcor holds no AVM value for that PID — it is *not* a failed check, and `avm_exceeds_ltv` carries no meaning in that case. Never report a pass or fail without checking `has_avm` first.
5. **Pull the report for the file.** `read_property_pdf_property__pid__report_pdf_get` (`GET /property/{pid}/report/pdf`) returns a `PropertyReportPdfResponse`: `pid`, `aa_code`, `j_code`, `roll_number`, `encrypted_pdf` and `valuation`. `encrypted_pdf` is a base64-encoded, **password-protected** PDF inside a JSON envelope — not an `application/pdf` body, and the API does not hand you the password. Pass the document through to the human who has it; do not attempt to read it.

## Rules

- **`POST` here does not mean "create".** Both POST operations in this API are stateless computations — `run_ltv_check` creates nothing and `generate-avm-summary` only narrates a payload you supply. There is no idempotency key because there is nothing to duplicate, but there is also no published retry-safety guarantee, so treat repeat calls as fresh billable reads.
- **The PDF path has two undeclared failure modes.** Per the spec's own description, a `404` means the PID could not be resolved to assessment area, jurisdiction and roll number, and a `502` means the legacy Landcor SOAP webservice behind the report rejected the request or returned nothing usable. Neither is declared as a response object. Retry a `502`; do not retry a `404`.
- **Validation.** A malformed `pid` returns `422` with `{"detail": [{loc, msg, type}]}`. Match `^\d{3}-\d{3}-\d{3}$` before you call.
- **Licence terms bind the caller.** https://www.landcor.com/acceptable-use/ requires users not to stockpile data pulls and not to share licences or data pulls between users. Run the check for the file you are actually working, not as a batch sweep.
