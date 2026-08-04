---
name: Authenticate and pull the IRONSCALES incident queue
description: >-
  Exchange an IRONSCALES APP API Token for a JWT and page the phishing incident queue for a tenant,
  including unclassified incidents and scanback results.
api: openapi/ironscales-incident-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/_original/ironscales-management-api-openapi.json
operations:
  - get JWT token
  - Get list of Incidents
  - Get IDs list of unclassified incidents
  - Get list of Scanback Incidents
  - Get details of specific incident
  - List of escalated emails
---

# Authenticate and pull the IRONSCALES incident queue

Base URL: `https://appapi.ironscales.com/appapi`. All bodies and responses are `application/json`.

## Before you start

You need two things, both obtained from the IRONSCALES dashboard by a user holding the **ADMIN** or
**OWNER** role, under **Settings > Account Settings > General & Security**:

- the **APP API Token**
- the **Company ID** (`company_id`)

There is no operation that lists the companies a token can reach — `company_id` must be supplied by the
operator. Never guess it.

## Steps

1. **Get a JWT.** Call `get JWT token` (`POST /get-token/`) with the APP API Token. Send the returned
   JWT in the `Authorization` request header on every subsequent call. The specification does not publish
   a token lifetime, so re-run this step on a `400` ("Not authenticated") or `401`.
2. **Page the incident queue.** Call `Get list of Incidents` (`GET /incident/{company_id}/list/`). This
   operation supports both a page-number form (`page`, `page_size`) and an id-watermark form
   (`since_id`, `limit`), plus `period` / `start_time` / `end_time` windows and an
   `include_scanback` flag. Pick one paging form and stay on it — do not mix `since_id` with `page`.
3. **Find work that still needs a decision.** Call `Get IDs list of unclassified incidents`
   (`GET /incident/{company_id}/{status}/`) to get the id list for a given status rather than filtering
   the full queue client-side.
4. **Include the retrospective sweep when relevant.** `Get list of Scanback Incidents`
   (`GET /incident/{company_id}/scanback-list/`) returns incidents surfaced by scanning back over mail
   already delivered before IRONSCALES was connected.
5. **Read one incident.** Call `Get details of specific incident`
   (`GET /incident/{company_id}/details/{incident_id}`) for the full record before acting on it.
6. **Optional: user-reported mail.** `List of escalated emails` (`GET /emails/{company_id}/`) returns
   emails employees escalated, which is a different stream from clustered incidents.

## Rules

- **Rate limit: 120 requests per minute per company.** No `RateLimit-*` or `Retry-After` headers are
  published, so pace client-side; you only learn you are over budget when a `429` arrives.
- **`400` means "not authenticated" in this API**, not "malformed request" — re-auth before assuming a
  bad payload. `403` means the principal lacks rights for that procedure or that `company_id`.
- Error responses carry **no schema and no error code** — only the HTTP status is contractual. Do not
  parse an error body for a machine-readable reason.
