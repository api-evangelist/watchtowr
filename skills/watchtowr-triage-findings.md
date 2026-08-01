---
generated: '2026-07-21'
method: generated
name: Triage and retest security findings
description: Review open watchTowr findings, update their status, trigger a retest, and export evidence as PDF.
api: openapi/watchtowr-platform-openapi.yml
operations: [get_list_findings, get_finding_details, get_available_finding_statuses, update_finding_status, start_specific_finding_retest, export_pdf_for_finding]
source: operationIds verified verbatim in openapi/watchtowr-platform-openapi.yml
---

# Triage and retest security findings

Work the findings queue on a watchTowr Platform tenant.

## Auth
- Platform API key as a Bearer token, issued in the dashboard (Settings -> API Management / Integrations -> Client API). Host is per-tenant: `https://{tenant}.{region}.watchtowr.io`. See `authentication/watchtowr-authentication.yml`.

## Conventions
- Every list is offset-paginated: `page` (default 1) + `page_size` (default 10, max 30); read `meta.pagination.total_pages` to walk all pages. Errors come back as plain JSON `{message}` (`{message, errors}` on 422). See `conventions/watchtowr-conventions.yml`. No idempotency keys — do not blind-retry the retest POST.

## Steps
1. **List findings** — `get_list_findings` (`GET /api/client/findings/list`), paging with `page`/`page_size` until `current_page == total_pages`.
2. **Inspect one** — `get_finding_details` (`GET /api/client/findings/show/{id}`).
3. **Fetch valid statuses** — `get_available_finding_statuses` (`GET /api/client/findings/statuses`) before changing anything; only use a status this endpoint returns.
4. **Update status** — `update_finding_status` (`POST /api/client/findings/status/{id}`) with the chosen status in the JSON body. 422 means the body failed validation — read `errors`.
5. **Retest when remediated** — `start_specific_finding_retest` (`POST /api/client/findings/retest/{finding_id}`). This is a state-changing action; confirm the finding id first and do not repeat on timeout without checking the finding's detail.
6. **Export evidence** — `export_pdf_for_finding` (`GET /api/client/findings/export/{id}`) for reporting.

## Errors
- `401 {message: "Unauthenticated."}` — bad/expired key; `403` — key lacks permission; `404` — finding id not in this tenant. See `errors/watchtowr-problem-types.yml`.
