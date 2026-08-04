---
name: Run an IRONSCALES phishing simulation and training campaign
description: >-
  Build, approve, monitor and stop a security-awareness-training (SAT) phishing simulation campaign,
  including template selection, participant calculation, and per-user performance.
api: openapi/ironscales-sat-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/_original/ironscales-management-api-openapi.json
operations:
  - get JWT token
  - Get Campaign Setup
  - Get Templates List
  - Get Template Categories
  - Get landing pages
  - Get call for action pages
  - Get Trainings List
  - Get Training Providers
  - Get Training Preview URL
  - Calculate Participants
  - Search Participants
  - Create Draft Campaign
  - Approve Campaign
  - Get Campaign Details
  - Get Campaigns List
  - Stop Campaign
  - Delete Campaign
  - Get user campaign performance
  - Get campaign participants details
---

# Run an IRONSCALES phishing simulation and training campaign

## Steps

1. **Authenticate.** `get JWT token` (`POST /get-token/`).
2. **Read what the tenant allows.** `Get Campaign Setup` (`GET /sat/{company_id}/setup/`) returns the
   configuration envelope a campaign must fit inside. Start here, not with a hard-coded payload.
3. **Choose the content.** `Get Template Categories`
   (`GET /sat/{company_id}/templates/categories/`) then `Get Templates List`
   (`GET /sat/{company_id}/templates/`, paged with `page` / `items_per_page`, filterable by `locale_ids`).
   Pair it with `Get landing pages` (`GET /sat/{company_id}/landing-pages/`) and
   `Get call for action pages` (`GET /sat/{company_id}/cta/`).
4. **Choose the follow-up training.** `Get Training Providers`
   (`GET /sat/{company_id}/trainings/providers/`), `Get Trainings List`
   (`GET /sat/{company_id}/trainings/`), and `Get Training Preview URL`
   (`GET /sat/{company_id}/trainings/{training_id}/preview/`) to show a human what the learner will see.
5. **Scope the audience.** `Search Participants` (`POST /sat/{company_id}/participants/search/`) and
   `Calculate Participants` (`POST /sat/{company_id}/participants/`) resolve the target set — both are
   `POST` operations that read, so they are safe to repeat. `Get Participants List`
   (`GET /sat/{company_id}/participants/`) enumerates the result.
6. **Create the draft.** `Create Draft Campaign` (`POST /sat/{company_id}/campaigns/`). A draft sends
   nothing.
7. **Review, then approve.** `Get Campaign Details` (`GET /sat/{company_id}/campaigns/{campaign_id}/`),
   then `Approve Campaign` (`POST /sat/{company_id}/campaigns/{campaign_id}/approve/`).
8. **Monitor and, if needed, halt.** `Get Campaigns List` (`GET /sat/{company_id}/campaigns/`) and
   `Get Campaigns Lookup` (`GET /sat/{company_id}/campaigns/lookup/`) for the roster;
   `Stop Campaign` (`POST /sat/{company_id}/campaigns/{campaign_id}/stop/`) halts a running campaign;
   `Delete Campaign` (`DELETE /sat/{company_id}/campaigns/{campaign_id}/`) removes it.
9. **Report.** `Get user campaign performance`
   (`GET /mailboxes/{company_id}/user-campaigns-performance/`) gives per-employee results, and
   `Get campaign participants details` (`GET /campaigns/{company_id}/participants-details`) gives the
   participant-level view.

## Rules

- **`Approve Campaign` is the point of no return** — it sends simulated phishing mail to real employees.
  Never let an agent approve without explicit human sign-off, and never auto-retry it: it is a
  non-idempotent `POST` with no idempotency key, so a duplicate can double-send.
- Draft-then-approve is the safe pattern. Do all shaping in the draft.
- Paging here uses `page` + `items_per_page` (not `page_size`, which the incident endpoints use). Read the
  operation's own parameters; the API is not internally consistent on this.
- Stay inside 120 requests per minute per company.
