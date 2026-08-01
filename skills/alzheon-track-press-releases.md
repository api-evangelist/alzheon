---
name: Track Alzheon press releases and news
description: Poll Alzheon's corporate announcements — trial results, financings, regulatory milestones, conference presentations — incrementally from the anonymous WordPress REST API on alzheon.com, without refetching content you already have.
api: openapi/alzheon-content-openapi.yml
operations:
  - listPosts
  - getPost
  - listCategories
  - listTags
  - listMedia
generated: '2026-07-31'
method: generated
source: openapi/alzheon-content-openapi.yml + conventions/alzheon-conventions.yml
---

# Track Alzheon press releases and news

Alzheon is a private clinical-stage biopharmaceutical company. It runs **no developer API
programme** — no portal, no keys, no SDKs, no support channel. What it does serve is the
standard WordPress REST API on its corporate site, anonymously, at
`https://alzheon.com/wp-json`. That makes its dated announcements machine-readable.

Use this skill to keep a local copy of Alzheon's announcements current.

## Before you start

- **No authentication.** Every operation here is an anonymous `GET`. Do not look for an API
  key — none is issued. If a route returns `401` with `rest_forbidden`, it is an
  administrative route that is not part of the public surface; do not retry it.
- **Pace yourself.** `robots.txt` sets `Crawl-delay: 10`. Responses carry
  `Cache-Control: max-age=600`. One request per ten seconds, and do not re-poll inside a
  ten-minute window.
- **This is an incidental surface.** It carries no compatibility commitment and can be
  disabled without notice — see `lifecycle/alzheon-lifecycle.yml`. Re-read
  `GET /wp-json/` (`getApiIndex`) if anything 404s with `rest_no_route`.

## Steps

### 1. Establish the baseline

Call **`listPosts`** with `per_page=100` and `orderby=date`, `order=desc`.

```
GET https://alzheon.com/wp-json/wp/v2/posts?per_page=100&orderby=date&order=desc
```

Read `X-WP-Total` and `X-WP-TotalPages` from the response headers to size the job — 224
posts existed on 2026-07-31. Page with `page=2,3,…` until `page` exceeds `X-WP-TotalPages`,
or follow the `rel="next"` relation in the `Link` header. Stop at the boundary: paging past
it returns `400 rest_post_invalid_page_number`.

Record the highest `modified_gmt` you saw. That is your sync cursor.

### 2. Poll incrementally after that

Call **`listPosts`** with `modified_after` set to your cursor, and sort by modification time
ascending so you can advance the cursor as you go:

```
GET https://alzheon.com/wp-json/wp/v2/posts?modified_after=2026-07-16T16:13:24&orderby=modified&order=asc&per_page=100
```

This returns both new posts and edits to existing ones. An empty array means nothing changed
— that is the normal result, not an error. Advance the cursor to the highest `modified_gmt`
in the batch.

### 3. Reduce payload when you only need headlines

Add `_fields=id,slug,date,modified,link,title` to fetch a headline index cheaply. Fetch the
full body with **`getPost`** only for items you actually intend to read.

### 4. Resolve related objects in one round trip

Add `_embed` to inline the author, featured media and terms under `_embedded` instead of
issuing separate calls. If you prefer explicit calls, use **`listCategories`** and
**`listTags`** once to build id→name maps (10 categories, 28 tags on 2026-07-31), and
**`listMedia`** to resolve a `featured_media` id to its `source_url`.

### 5. Separate announcements from coverage

The site distinguishes Alzheon's own press releases from third-party coverage
(`/media/press-releases/` versus `/media/in-the-news/`). Use the category map from step 4 and
filter with `categories=<id>` rather than guessing from the title.

## Error handling

Errors use the WordPress envelope, **not** RFC 9457 — there is no `application/problem+json`
and no type URI. Read `data.status`, not a problem type:

```json
{"code":"rest_post_invalid_id","message":"Invalid post ID.","data":{"status":404}}
```

- `400 rest_invalid_param` — a parameter fell outside the declared type/enum/range.
  `per_page` maxes at 100; `orderby` must be one of the enumerated values.
- `404 rest_post_invalid_id` — the id does not exist. Re-list rather than incrementing ids.
- `401 rest_forbidden` — administrative route. Stop; do not retry.
- `5xx` — retry with exponential backoff from the ten-second baseline.

There is no idempotency key on this API and no rate-limit headers to read. Full catalogue in
`errors/alzheon-problem-types.yml`; full semantics in `conventions/alzheon-conventions.yml`.

## Handling the content responsibly

Posts describe **investigational** medicine. Valiltramiprosate (ALZ-801) is not an approved
product. When surfacing this content:

- Do not present pipeline, biomarker or trial content as approved medical advice or as a
  claim of efficacy.
- Keep the publication date attached — trial-stage statements go stale.
- Attribute Alzheon and link the canonical `link` field. Alzheon publishes a privacy policy
  but no terms of use and no API licence; the content is copyrighted. Do not redistribute
  wholesale.
