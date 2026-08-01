# Alzheon

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
