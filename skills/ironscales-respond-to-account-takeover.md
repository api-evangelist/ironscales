---
name: Respond to an IRONSCALES account takeover incident
description: >-
  Investigate an account takeover (ATO) incident, launch remediation for the compromised mailbox, and tune
  the tenant's ATO detection sensitivity.
api: openapi/ironscales-incident-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/_original/ironscales-management-api-openapi.json
operations:
  - get JWT token
  - Get Account Takeover incident details
  - Create Account Takeover remediation
  - Get account takeover sensitivity settings
  - Update account takeover sensitivity settings
  - Get the latest company impersonation incidents
---

# Respond to an IRONSCALES account takeover incident

## Steps

1. **Authenticate.** `get JWT token` (`POST /get-token/`).
2. **Investigate.** `Get Account Takeover incident details`
   (`GET /incident/{company_id}/account-takeover/{incident_id}/details/`) returns the signals behind the
   ATO determination for a specific mailbox.
3. **Corroborate.** `Get the latest company impersonation incidents`
   (`GET /mitigation/{company_id}/impersonation/details/`) shows whether the same identity is being
   impersonated externally, which changes the response posture.
4. **Remediate.** `Create Account Takeover remediation`
   (`POST /incident/{company_id}/account-takeover/{incident_id}/remediation/`) starts the remediation for
   the compromised account.
5. **Tune detection.** Read with `Get account takeover sensitivity settings`
   (`GET /settings/{company_id}/account-takeover/`), then write with
   `Update account takeover sensitivity settings` (`PUT /settings/{company_id}/account-takeover/`).

## Rules

- **`Create Account Takeover remediation` is the highest-consequence call in this API.** It acts on a real
  employee's mailbox and session. It is a non-idempotent `POST` with no dedup key. Require explicit human
  authorization, and on any timeout re-read `Get Account Takeover incident details` rather than resending.
- **`Update account takeover sensitivity settings` is tenant-wide.** Changing sensitivity changes detection
  for every mailbox in the company. Always `GET` the current settings first and echo the delta to a human
  before the `PUT`.
- `403` on these routes usually means the token's dashboard role is below ADMIN/OWNER, not that the
  incident is missing.
