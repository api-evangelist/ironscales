# IronScales

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

IRONSCALES is an AI-powered, API-based email security platform protecting organizations against phishing,
business email compromise (BEC), account takeover (ATO), VIP impersonation, QR-code phishing, malicious
URLs and attachments, and deepfake-assisted social engineering. Instead of sitting inline as a secure email
gateway, IRONSCALES connects to Microsoft 365 and Google Workspace through their APIs and works at the
mailbox level, with no MX record changes.

- Website: https://ironscales.com/
- API overview: https://ironscales.com/platform/api
- API reference: https://appapi.ironscales.com/appapi/docs/
- Status page: https://status.ironscales.com/
- Trust center: https://trust.ironscales.com/

## APIs profiled here

| API | Base URL | Contract |
|---|---|---|
| IRONSCALES Management API | `https://appapi.ironscales.com/appapi` | Swagger 2.0, 46 paths / 61 operations / 129 definitions |
| IRONSCALES MCP Server | `https://mcp.ironscales.com/mcp/` | Remote MCP over streamable HTTP, OAuth 2.0 protected |

The Management API contract is published by the provider at
`https://appapi.ironscales.com/appapi/docs/?format=openapi` (a drf-yasg schema view; note it only responds
to `Accept: */*` or `application/openapi+json`). The verbatim document is kept in
`openapi/_original/`, and `openapi/*.yml` are one-per-tag splits of it.

## Artifacts

| Directory | What it holds |
|---|---|
| `openapi/` | The provider's Swagger 2.0 contract, verbatim plus one file per tag |
| `mcp/` | The live, OAuth-protected MCP server manifest and the REST/MCP tool crosswalk |
| `well-known/` | RFC 8414 authorization-server and RFC 9728 protected-resource metadata, plus the full probe log |
| `authentication/` | The JWT (REST) and OAuth 2.0 + PKCE (MCP) credential models |
| `conventions/` | Pagination, tenancy, versioning, error envelope, and the absence of an idempotency contract |
| `errors/` | Every 4xx/5xx the contract declares, and the fact that none of them carries a schema |
| `rate-limits/` | The documented 120 requests / minute / company throttle |
| `lifecycle/` | Versioning, status page, support, and the missing deprecation policy |
| `changelog/` | The product release train and seasonal releases |
| `conformance/` | Standards conformance, plus the published certifications |
| `security/` | Domain security probe, trust center, and responsible disclosure |
| `data-model/` | Entity graph derived from the contract's 129 definitions |
| `skills/` | Six packaged Agent Skills, every operationId verified against the spec |
| `llms/` | The provider's own `llms.txt`, saved verbatim |
| `overlays/` | Our annotations over the contract, without mutating the original |

## Notable gaps

These are recorded honestly in the artifacts, not papered over:

- No `security.txt` on any host, although a responsible-disclosure route and contact do exist on the trust center.
- No A2A agent card at either well-known path.
- No idempotency contract, on an API whose writes act on real employee mailboxes.
- No schema on any error response — only the HTTP status is contractual.
- No deprecation or sunset policy, while a V1 and V2 of the mitigation statistics operation both run.
- No first-party SDK in any public package registry.
- No AsyncAPI or webhook contract; the deepfake SIEM event feed is pull-only.
