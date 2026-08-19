---
name: Launch an AppLovin campaign with creatives
description: Create an Axon advertising campaign, upload creative assets, assemble them into a creative set, and verify the campaign is live — the full advertiser onboarding flow on the Campaign Management API.
api: openapi/applovin-campaigns-api-openapi.yml
operations:
  - createCampaign
  - uploadAssets
  - getAssetUploadResult
  - createCreativeSet
  - listCreativeSetsByCampaignId
  - listCampaigns
generated: '2026-08-13'
method: generated
source: https://support.applovin.com/en/app-discovery/api/axon-campaign-management-api/
---

# Launch an AppLovin campaign with creatives

This flow spends real money. AppLovin publishes no sandbox for the Campaign Management
API — there is no test host and no test key — so the first call you make is against a live
advertising account. Read the guardrails before the steps.

## Guardrails — read first

- **Never retry a 4xx.** The Campaign Management API blocks the entire account for
  **24 hours** after 100 error responses in a five-minute window, returning 429 with
  header `x-al-error-code: 100007`. A retry loop reaches that threshold in seconds. Fail
  closed on the first 4xx and surface it.
- **Asset upload failures count as errors.** Each file that comes back with
  `file_status: FAILURE` counts as one error against that budget. A single bad
  multi-asset upload can burn a large share of it.
- **No idempotency.** There is no `Idempotency-Key` header on this API. A retried
  `createCampaign` creates a **second campaign** with a second budget. If a request times
  out, call `listCampaigns` to find out what happened — do not resend.
- **Rate limit is 1,000 requests / 60 seconds**, account-wide. No `RateLimit-*` headers
  and no `Retry-After` are returned, so you cannot see your remaining budget. Pace
  yourself; do not discover the limit by hitting it.

## Authentication

Base URL: `https://api.ads.axon.ai/manage/v1`

Every request needs two things:

1. `Authorization: «campaign-management-API-key»` — the raw key, **no `Bearer ` prefix**.
2. `account_id=«account-ID»` as a **query parameter**, on every single call.

The Campaign Management API key is **not** the Management Key used by the MAX Ad Unit
Management API, and it is not the Report Key. All three are issued on the same dashboard
page (Account > General > Keys) and are not interchangeable. A 401 on a request that looks
correct is almost always the wrong key for this product.

## Steps

### 1. Create the campaign — `createCampaign`

`POST /campaign/create?account_id=«account-ID»`

Body is a `Campaign` object. The fields that decide whether the campaign can actually
serve are:

- `name`, `type`, `status`
- `platform` and `package_name` (plus `itunes_id` on iOS) — what you are promoting
- `start_date` / `end_date`
- `bidding_strategy`
- `budget` — a `Budget` with either `daily_budget_for_all_countries` or a
  `country_code_to_daily_budget` map
- `goal` — a `Goal` with `goal_type`, `goal_value_for_all_countries` or
  `country_code_to_goal_value`, and `roas_day_target` for ROAS goals
- `targeting` — `country_code`, `region_codes`, `metro_names`
- `tracking` — `tracking_method`, `impression_url`, `click_url`. This is where your MMP
  (Adjust, AppsFlyer, Singular) template goes. Get it right at creation; attribution gaps
  are not backfillable.

Keep the returned `id`. You will need it for every subsequent step.

### 2. Upload creative assets — `uploadAssets`

`POST /asset/upload?account_id=«account-ID»`

This is **asynchronous**. The response is a receipt, not a result.

### 3. Poll the upload — `getAssetUploadResult`

`GET /asset/upload_result?account_id=«account-ID»`

Returns an `AssetUploadResult` with `upload_status`, a `summary`, and a `details` array of
`AssetUploadDetail` — one entry per file with `file_name`, `status`, `asset_id`, `message`.

Do not proceed until every asset you need shows a terminal status and carries an
`asset_id`. Any `FAILURE` here has already cost you an error against the account budget,
so read `message`, fix the file, and re-upload only the file that failed.

You can confirm the catalogue at any point with `listAssets`
(`GET /asset/list?account_id=«account-ID»`).

### 4. Assemble the creative set — `createCreativeSet`

`POST /creative_set/create?account_id=«account-ID»`

Body is a `CreativeSet`:

- `campaign_id` — the id from step 1
- `name`, `type`, `status`
- `assets` — the `asset_id` values from step 3
- `languages`, `countries` — must be consistent with the campaign's `targeting`, or the
  set will not serve everywhere you expect
- `product_page`

### 5. Verify — `listCreativeSetsByCampaignId`

`GET /creative_set/list_by_campaign_id?account_id=«account-ID»&campaign_id=«id»`

Returns a `CreativeSetsByCampaign` with `campaign_count`, `creative_set_count`, and
`campaigns`. Confirm your set is attached and its `status` is what you intended.

Then `GET /campaign/list?account_id=«account-ID»` to confirm the campaign's live state.

## Pagination when you list

`listCampaigns` and `listCreativeSets` use **page-number** pagination:

- `page` — first page is `1`
- `size` — maximum and default `100`

The response is a **bare JSON array**. There is no envelope, no total count, and no next
cursor. **An empty array is the only end-of-results signal.** Stop when you get one.

Filtering: `ids` or `hashed_ids`, comma-separated, maximum 100 values.
They are **mutually exclusive** — sending both is a 400, which costs you error budget.

## Reading errors

Error detail is in the **response headers**, not the body:

- `x-al-error-code` — the numeric AppLovin code
- `x-al-error-message` — the human-readable reason
- `X-TRACE-ID` — present on **every** response. Log it always; AppLovin support requires
  it on every ticket.

If you only parse the body you will see a status code and learn nothing.

## Reconcile afterwards

Campaign performance does not come from this API. Use the Growth Reporting API
(`getGrowthReport`, `GET https://r.applovin.com/report`) with the **Report Key** in the
`api_key` query parameter, and asset-level performance from `getAssetReport` /
`getAssetAnalyticsReport`. Join on `campaign_id`, `creative_set_id` and `asset_id`.
Note the 45-day request window — chunk longer ranges client-side.
