---
generated: '2026-07-21'
name: Inventory the external attack surface
method: generated
description: Walk every discovered asset class (domains, subdomains, IPs, ports, cloud storage, repos), drill into details, and submit new seeds for discovery.
api: openapi/watchtowr-platform-openapi.yml
operations: [get_list_asset_domains, get_list_asset_subdomains, get_list_asset_ips, get_asset_ip_details, get_asset_ip_ports, get_list_asset_cloud_storages, get_list_asset_repositories, get_list_certificates, submit_asset]
source: operationIds verified verbatim in openapi/watchtowr-platform-openapi.yml
---

# Inventory the external attack surface

Build a complete asset inventory from a watchTowr Platform tenant.

## Auth
- Bearer Platform API key on the per-tenant host `https://{tenant}.{region}.watchtowr.io`. See `authentication/watchtowr-authentication.yml`.

## Conventions
- All lists use `page`/`page_size` (max 30) with the `meta.pagination` envelope — loop until `current_page == total_pages`. See `conventions/watchtowr-conventions.yml`.

## Steps
1. **Root domains** — `get_list_asset_domains` (`GET /api/client/assets/domain/list`).
2. **Subdomains** — `get_list_asset_subdomains` (`GET /api/client/assets/subdomain/list`).
3. **IP addresses** — `get_list_asset_ips` (`GET /api/client/assets/ip/list`); for interesting hosts drill into `get_asset_ip_details` (`GET /api/client/assets/ip/show/{id}`) and enumerate open ports with `get_asset_ip_ports` (`GET /api/client/assets/ip/show/{id}/port/list`).
4. **Exposed storage and code** — `get_list_asset_cloud_storages` (`GET /api/client/assets/cloudStorage/list`) and `get_list_asset_repositories` (`GET /api/client/assets/repository/list`).
5. **Certificates** — `get_list_certificates` (`GET /api/client/certificates/list`) to catch expiring/misissued certs.
6. **Seed new scope** — `submit_asset` (`POST /api/client/seeddata`) to submit a newly acquired domain/IP for discovery. Write action: confirm scope ownership before submitting; 422 returns field-level `errors`.

## Errors
- Plain JSON `{message}` envelope; see `errors/watchtowr-problem-types.yml`.
