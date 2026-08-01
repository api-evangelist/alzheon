---
name: Harvest Alzheon science, pipeline and people pages
description: Pull Alzheon's durable corporate content — pipeline, mechanism of action, publications, posters, management team, board, scientific advisory board, expanded access policy — as structured objects rather than scraping rendered HTML.
api: openapi/alzheon-content-openapi.yml
operations:
  - listPages
  - getPage
  - searchContent
  - listMedia
  - getApiIndex
generated: '2026-07-31'
method: generated
source: openapi/alzheon-content-openapi.yml + data-model/alzheon-data-model.yml
---

# Harvest Alzheon science, pipeline and people pages

Alzheon's durable content — as opposed to its dated announcements — lives in WordPress
**pages**, not posts. There were 25 of them on 2026-07-31, covering the science (pipeline,
mechanism of action, publications, posters, conference symposia), the people (management
team, board of directors, scientific advisory board), the patients section (vision, expanded
access policy), careers, contact and the privacy policy.

Fetch them through the API instead of scraping the rendered site: you get clean titles,
slugs, parent/child structure, modification timestamps and the raw content block, with no
theme markup to strip.

## Before you start

No credential is required and none exists. Pace at one request per ten seconds
(`robots.txt` sets `Crawl-delay: 10`) and honour the ten-minute `Cache-Control` window. See
`conventions/alzheon-conventions.yml`.

## Steps

### 1. Confirm the surface is still there

Call **`getApiIndex`**:

```
GET https://alzheon.com/wp-json/
```

This is the provider's own route index. It is the authority on which routes exist — the
OpenAPI in this repository was derived from it. If a namespace or route you expect is gone,
the site's plugins changed; re-derive rather than assuming.

### 2. List the pages

Call **`listPages`** with a large page size and the hierarchy fields you need:

```
GET https://alzheon.com/wp-json/wp/v2/pages?per_page=100&orderby=title&order=asc
```

`X-WP-Total` tells you the collection size. Pages **nest** — `science/pipeline` is a child of
`science` — so read the `parent` field to rebuild the tree. `parent: 0` marks a top-level
page. Do not infer hierarchy from the URL path; use the field.

### 3. Fetch the pages you care about

Call **`getPage`** with the id from step 2, or filter the collection directly by slug:

```
GET https://alzheon.com/wp-json/wp/v2/pages?slug=pipeline
```

Slug filtering returns an array — take the first element, and handle the empty-array case,
which means the slug does not exist (you will not get a 404 here).

The `content.rendered` field carries HTML. `title.rendered` and `excerpt.rendered` carry
HTML-escaped entities — decode them before display.

### 4. Find content across both types at once

Call **`searchContent`** when you do not know whether something is a page or a post:

```
GET https://alzheon.com/wp-json/wp/v2/search?search=valiltramiprosate&per_page=50
```

This returns lightweight stubs — `id`, `title`, `url`, `type`, `subtype`. Take the `type` and
`id` and refetch with **`getPage`** or **`getPost`** for the full object. Narrow with the
`type` and `subtype` parameters when you want one kind only.

### 5. Collect the attached assets

Posters and publication assets are in the media library. Call **`listMedia`** filtered to a
parent object, or resolve a page's `featured_media` id:

```
GET https://alzheon.com/wp-json/wp/v2/media?parent=<page id>&per_page=100
```

Use `source_url` for the file, `mime_type` to distinguish PDFs from images, and `alt_text`
for accessible descriptions. 624 media items existed on 2026-07-31 — filter rather than
walking the whole library.

### 6. Re-harvest only what moved

Pages change rarely. Use `modified_after` with your last cursor and `orderby=modified`,
`order=asc`, exactly as in the press-release skill, rather than refetching all 25 each run.

## Error handling

Same WordPress envelope as the rest of this surface — `{code, message, data.status}`, not
RFC 9457. `400 rest_invalid_param` for a bad parameter, `404 rest_post_invalid_id` for a
missing id, `401 rest_forbidden` on an administrative route (stop, do not retry). Full
catalogue in `errors/alzheon-problem-types.yml`.

## Handling the content responsibly

The science pages describe an **investigational** agent. Valiltramiprosate (ALZ-801) has FDA
Fast Track designation but is not approved for marketing. Do not restate mechanism-of-action
or trial content as approved medical advice or as an efficacy claim, keep dates attached, and
attribute Alzheon with the canonical `link`. No API licence or terms of use is published; the
content is copyrighted.
