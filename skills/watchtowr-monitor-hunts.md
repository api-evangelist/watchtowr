---
generated: '2026-07-21'
method: generated
name: Monitor rapid-reaction hunts
description: Track watchTowr hunts for emerging threats and pull the affected assets and findings for each.
api: openapi/watchtowr-platform-openapi.yml
operations: [get_client_hunts, show_the_detail_hunt, get_list_asset_by_hunt, get_list_finding_by_hunt, get_list_suspicious_domain]
source: operationIds verified verbatim in openapi/watchtowr-platform-openapi.yml
---

# Monitor rapid-reaction hunts

When watchTowr launches a hunt for an emerging CVE, report what it touched on your tenant.

## Auth
- Bearer Platform API key on the per-tenant host. See `authentication/watchtowr-authentication.yml`.

## Steps
1. **List hunts** — `get_client_hunts` (`GET /api/client/hunts/list`), newest first; page with `page`/`page_size` (max 30).
2. **Hunt detail** — `show_the_detail_hunt` (`GET /api/client/hunts/show/{id}`).
3. **Affected assets** — `get_list_asset_by_hunt` (`GET /api/client/hunts/show/{id}/assets`).
4. **Resulting findings** — `get_list_finding_by_hunt` (`GET /api/client/hunts/show/{id}/findings`); hand confirmed findings to the triage skill (`skills/watchtowr-triage-findings.md`).
5. **Check brand exposure** — `get_list_suspicious_domain` (`GET /api/client/suspicious-domain/list`) for lookalike domains registered around the same window.

## Conventions and errors
- Offset pagination + `meta.pagination` envelope (`conventions/watchtowr-conventions.yml`); `{message}` error envelope (`errors/watchtowr-problem-types.yml`); 404 means the hunt id is not visible to this tenant.
