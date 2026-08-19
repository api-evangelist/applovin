---
name: Configure a MAX ad unit waterfall
description: Create or update a MAX ad unit, set per-segment waterfall network settings and bid floors, and register a test device to verify the integration on the Ad Unit Management API.
api: openapi/applovin-ad-units-api-openapi.yml
operations:
  - listAdUnits
  - getAdUnit
  - createAdUnit
  - updateAdUnit
  - getAdUnitWaterfall
  - updateAdUnitWaterfall
  - createTestDevice
  - listTestDevices
generated: '2026-08-13'
method: generated
source: https://support.applovin.com/en/max/advanced-features/ad-unit-management-api
---

# Configure a MAX ad unit waterfall

This is the publisher-side monetization flow. It changes what ads real users see and what
revenue the app earns, immediately, with no staging environment.

## Guardrails — read first

- **There is no sandbox.** No test host, no test key. `updateAdUnitWaterfall` takes effect
  on live traffic.
- **No idempotency.** No `Idempotency-Key` header. A retried `createAdUnit` creates a
  second ad unit. If a call times out, `listAdUnits` before you resend.
- **Rate limit is 2,000 requests / hour**, account-wide. No rate-limit headers are
  returned; you cannot see your remaining budget.
- **Read before you write.** `updateAdUnitWaterfall` replaces the segment's configuration.
  `getAdUnitWaterfall` first, mutate the object you got back, then send it — do not
  construct one from scratch.

## Authentication

Base URL: `https://o.applovin.com/mediation/v1`

`Api-Key: «management-key»` — your account's **Management Key**, from the AppLovin
dashboard under Account > General > Keys.

This is a different credential from the Campaign Management API key and from the Report
Key. Note also that the Ad Review Rules API uses the same header *name* (`Api-Key`) on a
different host with a different key value — do not cross them.

## Steps

### 1. Find the ad unit — `listAdUnits` / `getAdUnit`

`GET /ad_units` returns every ad unit as `AdUnitSummary` (`id`, `name`, `platform`,
`ad_format`, `package_name`, `disabled`). There is **no pagination** on this collection;
you get all of them.

`GET /ad_unit/{ad_unit_id}` returns the full `AdUnit`. The id is a 16-character lowercase
hex string, e.g. `deb878533ea4e76a`. Keep it — the same value appears as `max_ad_unit_id`
in the revenue report, and it is the **only** join key between mediation config and
reporting.

Check `has_active_experiment` before changing anything. If an A/B experiment is running,
editing the base waterfall changes the control arm underneath a live test.

### 2. Create an ad unit if you need one — `createAdUnit`

`POST /ad_unit`

Body fields: `name`, `platform`, `package_name`, `ad_format` (`INTER`, `BANNER`,
`REWARD`), `template_size`.

A 403 here means the account does not own the app you named.

### 3. Set the network configuration — `updateAdUnit`

`POST /ad_unit/{ad_unit_id}`

The `AdUnit` payload carries the monetization levers:

- `ad_network_settings` — an array of `AdNetworkSetting`: `ad_network`, `cpm`,
  `country_targeting_type`, `countries`, and `ad_network_ad_units` (each an
  `AdNetworkAdUnit` with the **third-party network's** `ad_network_ad_unit_id` — an
  external identifier AppLovin stores but does not mint, so a typo here fails silently as
  no fill rather than as an API error)
- `disabled_ad_network_settings` — networks kept but turned off
- `bid_floors` — `BidFloor` entries with `country_group_name`, `cpm`, `countries`
- `frequency_capping_settings` — `day_limit`, `minute_frequency`, `session_limit`
- `banner_refresh_settings` / `mrec_refresh_settings`
- `segments` — `Segment` entries with `id`, `name`, `id_type`, `device_type`,
  `segment_keys`

### 4. Set the per-segment waterfall — `getAdUnitWaterfall` then `updateAdUnitWaterfall`

`GET /ad_unit/{ad_unit_id}/{segment_id}` returns a `Waterfall`: `ad_unit_id`,
`segment_id`, `ad_network_settings`, `bid_floors`.

`POST /ad_unit/{ad_unit_id}/{segment_id}` creates, edits, deprecates, promotes, or removes
that segment's waterfall.

A `Waterfall` has **no id of its own** — it is addressed only by the
(`ad_unit_id`, `segment_id`) pair. There is no way to reference one otherwise.

`deprecate` and `promote` here are lifecycle verbs on *your* configuration. They are not
AppLovin deprecating an API; AppLovin publishes no deprecation policy at all.

### 5. Register a test device — `createTestDevice`

`POST /test_device`

`TestDevice` carries `name`, `device_id`, `network`, `disabled`. `device_id` is a **real
device advertising identifier** — a GAID on Android, an IDFA on iOS — supplied by you.
AppLovin publishes no synthetic test identifiers.

`GET /test_devices` lists them; `GET /test_device/{test_device_id}` reads one;
`POST /test_device/{test_device_id}` updates one.

Registering a test device here is equivalent to calling
`setTestDeviceAdvertisingIds(...)` in the SDK initialization config, and it is the only
part of the HTTP surface that touches testing.

### 6. Verify on the device

Open the **Mediation Debugger** in the app and enable test ads, then confirm
`Test Mode On: true` appears in the SDK initialization logs.

Two things that will mislead you:

- AppLovin does **not** track impressions, clicks, or revenue for test ads, so test
  traffic never reaches the reporting APIs. You cannot validate this flow end-to-end by
  watching a report.
- Server-to-server rewarded callbacks are **not sent for test ads**, only live ads. The
  rewarded callback path is unverifiable in test mode.

No-fills during testing are common and usually do not indicate a broken integration.

## Running an A/B test instead

To test a waterfall change rather than ship it, use the experiment operations against
`/ad_unit_experiment/{ad_unit_id}`: `getAdUnitExperiment`, `upsertAdUnitExperiment`, and
`getAdUnitExperimentWaterfall` / `updateAdUnitExperimentWaterfall` for the per-segment arm.

An ad unit has **at most one active experiment**. `upsertAdUnitExperiment` creates one
only if none is active; otherwise it promotes or deprecates the existing one via the
`promote` / `deprecate` fields on the `AdUnitExperiment` payload.

## Reconcile revenue afterwards

Mediation revenue comes from a different host with a different key: `getMaxRevenueReport`,
`GET https://r.applovin.com/maxReport`, authenticated with the **Report Key** in the
`api_key` **query parameter**.

Join on `max_ad_unit_id`. Note the 45-day request window, and that aggregated data from
the last 1–2 hours may be incomplete while it is processed.
