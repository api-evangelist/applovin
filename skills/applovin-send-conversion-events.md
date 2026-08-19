---
name: Send web conversion events to AppLovin
description: Submit page_view and generate_lead events server-side through the AppLovin Conversion API, de-duplicated against the browser pixel — the one AppLovin write surface with a real idempotency key.
api: openapi/applovin-conversion-events-api-openapi.yml
operations:
  - postConversionEvents
generated: '2026-08-13'
method: generated
source: https://support.applovin.com/en/growth/promoting-your-websites/api/conversion-api-for-lead-gen
---

# Send web conversion events to AppLovin

Server-side conversion tracking for web advertisers. AppLovin recommends running this
**alongside** the AppLovin Pixel, not instead of it — the two are de-duplicated by a key
you supply.

This endpoint carries personal data. Read the PII section before you send anything.

## Authentication

`POST https://b.applovin.com/v1/event?pixel_id=«event-key»`

Two credentials, both from the dashboard Keys page, and both required:

1. `Authorization: «conversion-API-key»` — the header credential
2. `pixel_id=«event-key»` — your AppLovin **Event Key**, as a query parameter

These are two different values. The Event Key is the same one used as the hashing secret
for server-to-server rewarded callbacks.

## Response codes

| Code | Meaning |
|---|---|
| 200 | All events processed successfully |
| 400 | Error in the request; **the entire batch is dropped** |
| 401 | Authentication failed |

## Guardrails — read first

- **Batches are all-or-nothing.** "All events must be valid. Any invalid event will fail
  the batch." One malformed event in a batch of 100 loses all 100. Validate client-side
  before sending; never assume partial success.
- **Maximum batch size is 100 events.**
- **Do use `dedupe_id`.** It is the only idempotency mechanism AppLovin publishes anywhere
  in its API surface, and it is what makes a retry safe here.
- **The retention window for `dedupe_id` is not published.** You cannot know how long a
  replay stays de-duplicated, so treat a same-day retry as safe and a delayed replay as
  unknown.

## Request body

```
{
  "events": [ ServerEvent, ... ]     // max 100
}
```

### ServerEvent

| Field | Type | Required | Notes |
|---|---|---|---|
| `name` | string | yes | Must be exactly `page_view` or `generate_lead`. No other values. |
| `event_time` | number | yes | Unix epoch in **milliseconds**, not seconds. |
| `event_source_url` | string | yes | The complete URL where the event occurred, including query string. |
| `data` | EventData | yes | See below. |
| `user_data` | UserData | yes | See below. |
| `dedupe_id` | string | no | Unique id for this event. **Send it.** |

### EventData

- For `page_view`: set `data` to `null`.
- For `generate_lead`: always send `{ "currency": "«ISO-4217»", "value": «number» }`.
  `value` is the monetary worth of the lead to your business — pass a higher value for a
  higher-quality lead, since it is what the optimizer bids against.

### UserData — this is personal data

`aleid`, `axwrt`, `alart`, `client_id`, `user_id`, `email`, `phone`,
`client_ip_address`, `client_user_agent`, `esi`, `country_code`, `zip`, `os`, `sid`,
`ifa`, `idfv`.

This is the highest-sensitivity schema in the AppLovin surface: contact identifiers, IP
address, user agent, and device advertising identifiers. Before an agent sends any of it:

- Confirm you have a lawful basis and consent for the identifiers you are transmitting.
- Send the AppLovin-native identifiers (`aleid`, `axwrt`, `alart`, `client_id`) in
  preference to raw contact identifiers where the match can be made without them.
- Follow AppLovin's documented hashing requirements for `email` and `phone`. Do not
  invent a hashing scheme.

## De-duplication — the point of `dedupe_id`

The browser pixel and this server-side call will both fire for the same user action. Send
**the same `dedupe_id` on both** so AppLovin counts one conversion, not two.

`dedupe_id` is explicitly **not** the same thing as `aleid` in `user_data`. `aleid`
identifies the user; `dedupe_id` identifies the event.

Practically: derive it from something stable and unique in your own system — a form
submission id, an order id — and emit the identical value from your pixel `<script>` tag
and from your server call.

## Retry behaviour

- On a **timeout or 5xx**: retry the identical batch with the identical `dedupe_id`s. That
  is what the key is for.
- On a **400**: do **not** retry. The batch was rejected as malformed; resending sends the
  same malformed batch. Find the invalid event, fix it, resend.
- On a **401**: do not retry. Check which of the two credentials is wrong — the
  `Authorization` header key or the `pixel_id` Event Key.

## Reconcile afterwards

Web campaign performance comes from the Web Reporting API,
`GET https://r.applovin.com/webReport`, with the **Report Key** as the `api_key` query
parameter and `report_type=advertiser`. It accepts an `attribution_mode` of `click` or
`click_and_view`.

Note the window here is **90 days**, not the 45 days that applies to the MAX, growth,
asset and cohort reports.
