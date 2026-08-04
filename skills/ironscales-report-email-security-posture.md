---
name: Report IRONSCALES email security posture
description: >-
  Pull the tenant-level mitigation, email and targeting statistics that make up an IRONSCALES email
  security report, plus the mailbox compliance report and deepfake event feed.
api: openapi/ironscales-mitigation-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/_original/ironscales-management-api-openapi.json
operations:
  - get JWT token
  - Get a company mitigation statistics V2
  - Get a company mitigation statistics
  - Get emails stats
  - Most targeted employees
  - Most targeted departments
  - Get a company mitigation details
  - Get compliance report
  - List of a company mailboxes
  - Get list of Deepfake SIEM events
---

# Report IRONSCALES email security posture

## Steps

1. **Authenticate.** `get JWT token` (`POST /get-token/`).
2. **Headline numbers.** `Get a company mitigation statistics V2`
   (`GET /mitigation/{company_id}/stats/v2/`). Prefer the V2 form. The V1 form,
   `Get a company mitigation statistics` (`GET /mitigation/{company_id}/stats/`), still exists and is not
   marked deprecated — IRONSCALES publishes no deprecation policy, so treat V1 as legacy-but-live and do
   not build new reporting on it.
3. **Volume breakdown.** `Get emails stats` (`GET /mitigation/{company_id}/stats/emails/`).
4. **Who is being attacked.** `Most targeted employees`
   (`GET /mitigation/{company_id}/stats/most-targeted-employees/`) and `Most targeted departments`
   (`GET /mitigation/{company_id}/stats/most-targeted-departments/`).
5. **Drill down.** `Get a company mitigation details`
   (`GET /mitigation/{company_id}/incidents/details/`) for the incident-level backing data.
6. **Coverage.** `List of a company mailboxes` (`GET /mailboxes/{company_id}/list/`) and
   `Get compliance report` (`GET /mailboxes/{company_id}/compliance-report/`) show which mailboxes are
   protected and their compliance state.
7. **Deepfake signal.** `Get list of Deepfake SIEM events` (`GET /deepfake/{company_id}/events/`) is the
   feed intended for SIEM ingestion — it is **pull-only**. IRONSCALES publishes no webhook or AsyncAPI
   event contract, so an agent must poll this on a schedule and track its own watermark.

## Rules

- Every statistics operation takes a time window (`period`, or `start_time` / `end_time`, or
  `customPeriodFrom` / `customPeriodTo` depending on the operation). Read each operation's own parameters
  — the window parameter names are not uniform.
- These are all reads: safe to repeat, safe to run unattended.
- Stay inside 120 requests per minute per company; a wide report across many tenants will hit that ceiling
  first, and there is no header telling you how much budget is left.
