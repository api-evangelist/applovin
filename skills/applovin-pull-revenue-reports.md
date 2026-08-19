---
name: Pull AppLovin revenue and performance reports
description: Retrieve MAX mediation revenue, campaign performance, asset-level performance and user-level ad revenue from the AppLovin reporting family, respecting the request windows and the query-string credential.
api: openapi/applovin-revenue-reporting-api-openapi.yml
operations:
  - getMaxRevenueReport
  - getGrowthReport
  - getAssetReport
  - getAssetAnalyticsReport
generated: '2026-08-13'
method: generated
source: https://support.applovin.com/en/max/reporting-apis/revenue-reporting-api
---

# Pull AppLovin revenue and performance reports

Read-only. This is the safest part of the AppLovin surface and the one an agent is most
likely to be asked for.

## Authentication — and its one real hazard

Every reporting endpoint authenticates with the **Report Key** as the `api_key`
**query parameter**:

```
GET https://r.applovin.com/maxReport?api_key=«report-key»&...
```

There is no header alternative. This means a long-lived, account-wide credential is
written into the URL of every request, and therefore into web server access logs, proxy
logs, CDN logs, and shell history. Treat any log line containing a reporting URL as a
secret. Redact `api_key` before writing a request URL into any transcript, ticket, or
trace.

The Report Key is not the Management Key and not the Campaign Management API key.

## The endpoints

| Report | Endpoint | Window |
|---|---|---|
| MAX mediation revenue | `GET https://r.applovin.com/maxReport` | 45 days |
| Growth (campaign) performance | `GET https://r.applovin.com/report` | 45 days |
| Asset performance, fixed range | `GET https://r.applovin.com/assetReport` | 45 days |
| Asset performance, date range | `GET https://r.applovin.com/assetAnalyticsReport` | 45 days |
| Web campaign performance | `GET https://r.applovin.com/webReport` | **90 days** |
| Install-cohorted revenue | `GET https://r.applovin.com/maxCohort` | 45 days |
| Cohorted impressions | `GET https://r.applovin.com/maxCohort/imp` | 45 days |
| Cohorted sessions | `GET https://r.applovin.com/maxCohort/session` | 45 days |
| User-level ad revenue | `GET https://r.applovin.com/max/userAdRevenueReport` | 45 days |

**The request window is a hard bound, not a truncation.** A request whose `start`/`end`
falls outside it fails. Chunk long ranges client-side into window-sized slices — this is
the single most common integration error against these APIs.

## Common parameters

- `api_key` — Report Key (required)
- `columns` — comma-separated list of columns; you choose the projection
- `start` / `end` — `YYYY-MM-DD` (the growth and web reports also accept a Unix timestamp,
  and `end=now`)
- `format` — `json` or `csv`. This is a **query parameter, not an `Accept` header**; there
  is no content negotiation.
- `limit` / `offset` — pagination
- `sort_«column»` — `ASC` or `DESC`; multiple sorts apply in query-string order
- `filter_«column»` — exact-match filter on a column
- `not_zero=1` — drop rows where every numeric metric is 0
- `having` — a URL-encoded numeric expression, e.g.
  `impressions%20%3E%200%20AND%20revenue%20%3E%200`. **AppLovin's own docs warn this slows
  the response and increases the likelihood of timeouts.** Prefer filtering client-side.

### Growth and web reports only

- `report_type` — `publisher` or `advertiser`. The growth report **defaults to
  `publisher`**; the web report **requires `advertiser`**. Getting this wrong returns a
  valid-looking report about the wrong side of the business.
- `day_column=day` — switch metrics from realtime to cohorted (attributed back to serve
  time). Realtime is the default, and realtime and cohorted numbers for the same range
  will not match. Say which one you pulled.
- `attribution_mode` (web only) — `click` or `click_and_view`.

## `getMaxRevenueReport` — the MAX revenue report

`GET /maxReport`

Returns aggregated estimated MAX mediation statistics. **All times in the response are
UTC.**

Useful columns: `day`, `hour`, `application`, `package_name`, `ad_format`
(`INTER` / `BANNER` / `REWARD`), `country` (two-letter ISO), `device_type`
(`phone` / `tablet` / `other`), `network`, `max_ad_unit`, `max_ad_unit_id`, `impressions`,
`clicks`, `ctr`, `ecpm`, `estimated_revenue`, `has_idfa`.

Two caveats worth surfacing to whoever asked for the numbers:

- **Aggregated data from the last 1–2 hours may be incomplete** while it is processed.
  Never present the trailing two hours as final.
- `attempts` and `fill_rate` are only returned when you include `network` and/or
  `network_placement`, and are **not available** alongside `max_placement`. Some column
  combinations are mutually exclusive.

`max_ad_unit_id` joins back to `AdUnit.id` in the Ad Unit Management API. It is the only
key linking mediation configuration to mediation revenue.

## `getGrowthReport` — campaign performance

`GET /report`

Advertiser or publisher campaign data. `campaign_id_external` is the stable campaign
reference — it does not change when a campaign is renamed, and it equals the
`{CAMPAIGN_ID}` click macro. Join on it rather than on `campaign` (the name).

## `getAssetReport` / `getAssetAnalyticsReport` — creative performance

- `GET /assetReport` takes `range`: `yesterday`, `last_7d`, or `last_month`.
- `GET /assetAnalyticsReport` takes `start` and `end` dates.

Both require `columns`. Available: `asset_id`, `asset_name`, `asset_url`, `campaign`,
`campaign_id`, `campaign_package_name`, `creative_set`, `creative_set_id`, `clicks`,
`cost`, `ctr`, `impressions`.

Filterable on `asset_id`, `asset_name`, `campaign`, `campaign_id`,
`campaign_package_name`, `creative_set`, `creative_set_id` — direct match only.

**Data for yesterday is stable after 06:00 UTC.** Pull before that and you get a partial day.

## User-level ad revenue — it returns a link, not rows

`GET /max/userAdRevenueReport?api_key=«report-key»&date=«YYYY-MM-DD»&platform=«android|ios|fireos»&application=«package»&aggregated=«true|false»`

The response body is **not** the data. It is `{ "status": 200, "url": "https://...s3.amazonaws.com/...csv?X-Amz-..." }` —
a presigned S3 URL you then fetch to get the CSV.

- `aggregated=true` (default) gives one row per user; `false` gives one row per impression.
- `application` and `store_id` are **mutually exclusive** — send one, never both.
- Data is available **eight hours after UTC day end**. Data for 2019-01-01 lands
  2019-01-02 after 08:00.

Treat the presigned URL as a short-lived secret and do not log it.

## Pagination

`limit` and `offset`. The Cohort API adds a caveat the others do not: **set `offset=0`
explicitly on the first request** to preserve result ordering across pages.

## Rate limits and errors

AppLovin publishes **no** rate limit for any reporting endpoint, and returns no
`RateLimit-*` headers and no `Retry-After`. Absence of a published limit is not a promise
of no limit — pace requests conservatively.

There is no published error schema for the reporting hosts. On failure, read
`x-al-error-code` and `x-al-error-message` from the response headers, and always log
`X-TRACE-ID`, which is returned on every response and which AppLovin support requires on
every ticket.
