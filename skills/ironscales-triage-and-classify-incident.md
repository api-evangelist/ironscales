---
name: Triage and classify an IRONSCALES phishing incident
description: >-
  Read a clustered phishing incident, classify it, split or re-merge a mis-clustered cluster, and check
  remediation status across the affected mailboxes.
api: openapi/ironscales-incident-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/_original/ironscales-management-api-openapi.json
operations:
  - get JWT token
  - Get details of specific incident
  - Classify specific incident
  - Recluster incident
  - Uncluster incident
  - Remediation statuses stats
  - Get details of mitigations per mailbox
---

# Triage and classify an IRONSCALES phishing incident

## Steps

1. **Authenticate.** `get JWT token` (`POST /get-token/`); send the JWT in `Authorization`.
2. **Read the incident.** `Get details of specific incident`
   (`GET /incident/{company_id}/details/{incident_id}`). Do this before every write — there is no
   conditional-request or ETag support, so the read is your only concurrency check.
3. **Classify it.** `Classify specific incident`
   (`POST /incident/{company_id}/classify/{incident_id}`) records the verdict, which drives IRONSCALES'
   automated remediation across every affected mailbox.
4. **Fix a bad cluster, if needed.** `Uncluster incident`
   (`POST /incident/{company_id}/uncluster/{incident_id}`) splits emails wrongly grouped into one
   incident; `Recluster incident` (`POST /incident/{company_id}/recluster/{incident_id}`) re-groups them.
   These change what a subsequent classification applies to, so run them **before** classifying.
5. **Confirm the outcome.** `Remediation statuses stats`
   (`GET /incident/{company_id}/stats/remediation-statuses/`) rolls up remediation state, and
   `Get details of mitigations per mailbox` (`POST /mitigation/{company_id}/details/`) shows what actually
   happened per mailbox.

## Rules

- **These POSTs are not idempotent and there is no `Idempotency-Key` header.** On a timeout or `5xx`,
  do **not** retry blindly: re-read the incident with `Get details of specific incident` and only reissue
  the write if the verdict did not land. A double classify or a double recluster changes real mailbox
  state for real employees.
- Classification is a **consequential, tenant-visible action** — it triggers automated remediation.
  Require explicit human confirmation before an agent calls `Classify specific incident`.
- `Get details of mitigations per mailbox` is a `POST` that reads data (the filter travels in the body).
  Treat it as a read: it is safe to repeat.
- Stay inside 120 requests per minute per company.
