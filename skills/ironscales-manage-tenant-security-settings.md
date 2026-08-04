---
name: Manage IRONSCALES tenant security settings
description: >-
  Read and change a company's allow list, incident notification settings, and challenged-alert
  notification settings.
api: openapi/ironscales-settings-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/_original/ironscales-management-api-openapi.json
operations:
  - get JWT token
  - Get allow list settings
  - Create allow list settings
  - Update allow list entry
  - Delete allow list entries
  - Get company notification settings
  - Create company notification settings
  - Append to company notification settings
  - Delete company notification settings
  - Get challenged notification settings
  - Create challenged notification settings
  - Append challenged notification settings
  - Delete challenged notification settings
---

# Manage IRONSCALES tenant security settings

## Steps

1. **Authenticate.** `get JWT token` (`POST /get-token/`).
2. **Always read before you write.** Each settings family exposes a `GET` that returns the current state:
   `Get allow list settings` (`GET /settings/{company_id}/allow-list/`),
   `Get company notification settings` (`GET /settings/{company_id}/incident-alerts/`),
   `Get challenged notification settings` (`GET /settings/{company_id}/challenged-alerts/`).
3. **Allow list.** `Create allow list settings` (`POST /settings/{company_id}/allow-list/`),
   `Update allow list entry` (`PUT /settings/{company_id}/allow-list/`),
   `Delete allow list entries` (`DELETE /settings/{company_id}/allow-list/`).
4. **Incident alerts.** `Create company notification settings`
   (`POST /settings/{company_id}/incident-alerts/`), `Append to company notification settings`
   (`PUT /settings/{company_id}/incident-alerts/`), `Delete company notification settings`
   (`DELETE /settings/{company_id}/incident-alerts/`).
5. **Challenged alerts.** `Create challenged notification settings`
   (`POST /settings/{company_id}/challenged-alerts/`), `Append challenged notification settings`
   (`PUT /settings/{company_id}/challenged-alerts/`), `Delete challenged notification settings`
   (`DELETE /settings/{company_id}/challenged-alerts/`).

## Rules

- **The allow list is a security control.** Adding a sender or domain to it tells IRONSCALES to stop
  scrutinising that sender for the whole company. An agent must never add an allow-list entry without
  explicit, specific human approval of the exact value being allowed, and must never allow a wildcard or
  a free mail domain.
- **`DELETE /settings/{company_id}/allow-list/` deletes entries plural.** Read the current list, echo
  exactly which entries will be removed, and confirm before calling it.
- The `POST` create operations are not idempotent and there is no idempotency key. On a timeout, re-read
  with the matching `GET` and reconcile rather than resending — a resend can create a duplicate entry.
- Note the naming: on these routes `PUT` means **append**, not replace. Do not assume `PUT` overwrites the
  collection.
- Stay inside 120 requests per minute per company.
