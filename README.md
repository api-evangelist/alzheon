# Alzheon

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

Alzheon, Inc. is a privately held clinical-stage biopharmaceutical company founded in 2013
and headquartered in Framingham, Massachusetts, developing oral small-molecule therapeutics
and diagnostics for Alzheimer's disease and other neurodegenerative disorders. Its lead
candidate, valiltramiprosate (ALZ-801), is a valine-conjugated prodrug of tramiprosate that
blocks formation of neurotoxic soluble beta-amyloid oligomers; it holds FDA Fast Track
designation and completed the pivotal APOLLOE4 Phase 3 trial in APOE4/4 homozygotes with
early Alzheimer's disease.

- https://alzheon.com/
- https://forgeglobal.com/alzheon_stock/ (secondary-market listing — the original harvest source)

## API posture

**Alzheon operates no product or developer API.** There is no developer portal, no API
documentation, no SDKs, no CLI, no keys, no sandbox, no Postman collection, no status page,
no changelog, no MCP server and no A2A agent card. The `api.`, `developer.`, `docs.`,
`status.`, `trust.`, `security.` and `mcp.` subdomains do not resolve. No `/.well-known/`
document is served — every canonical path was probed and returned 404. The probe log is in
[`well-known/alzheon-well-known.yml`](well-known/alzheon-well-known.yml).

The one machine-readable contract the company serves is the standard **WordPress REST API**
on its corporate site, readable anonymously at `https://alzheon.com/wp-json`. It exposes 224
press releases and news items, 25 science/people/patients pages, 624 media items, taxonomies,
cross-content search and oEmbed. This is an incidental content-management surface, not a
first-party API product: Alzheon publishes no documentation, credentials or terms for it, and
all write and administrative routes return 401.

## Artifacts

| Artifact | File | Method |
|---|---|---|
| OpenAPI 3.1 (24 verified anonymous read operations) | [`openapi/alzheon-content-openapi.yml`](openapi/alzheon-content-openapi.yml) | derived from the live route index |
| Overlay of API Evangelist enhancements | [`overlays/alzheon-content-overlay.yaml`](overlays/alzheon-content-overlay.yaml) | generated |
| Authentication profile | [`authentication/alzheon-authentication.yml`](authentication/alzheon-authentication.yml) | derived |
| API conventions | [`conventions/alzheon-conventions.yml`](conventions/alzheon-conventions.yml) | derived |
| Error catalog | [`errors/alzheon-problem-types.yml`](errors/alzheon-problem-types.yml) | derived |
| Data model | [`data-model/alzheon-data-model.yml`](data-model/alzheon-data-model.yml) | derived |
| Lifecycle | [`lifecycle/alzheon-lifecycle.yml`](lifecycle/alzheon-lifecycle.yml) | derived |
| Conformance | [`conformance/alzheon-conformance.yml`](conformance/alzheon-conformance.yml) | derived |
| Well-known probe log | [`well-known/alzheon-well-known.yml`](well-known/alzheon-well-known.yml) | probed |
| Domain security | [`security/alzheon-domain-security.yml`](security/alzheon-domain-security.yml) | probed |
| Agent skills | [`skills/_index.yml`](skills/_index.yml) | generated |
| llms.txt | [`llms/alzheon-llms.txt`](llms/alzheon-llms.txt) | generated |

Not present, because the underlying thing does not exist: `packages/` (no first-party
library on npm, PyPI or any registry; no GitHub organisation), `mcp/`, `a2a/`, `asyncapi/`,
`cli/`, `components/`, `sandbox/`, `changelog/`, `scopes/` (no OAuth), `grpc/`, and the
vulnerability-disclosure and trust-center artifacts (probed; neither published).

## Note on content

Valiltramiprosate / ALZ-801 is an **investigational** agent that is not approved for
marketing. Content harvested through this repository must not be presented as approved
medical advice or as a claim of efficacy.
