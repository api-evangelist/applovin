# applovin

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

Profile of [AppLovin](https://www.applovin.com/) in the API Evangelist network. Fortune 2024 (rank 847).

AppLovin is a marketing platform that helps businesses reach, monetize, and grow their global audiences through mobile advertising, mediation, and analytics. The company operates platforms including AppDiscovery (Axon) for performance-based user acquisition, MAX for in-app bidding mediation, Adjust for mobile measurement, and Wurl for connected TV.

- Website: https://www.applovin.com
- Documentation: https://support.axon.ai
- Developer portal: https://developers.applovin.com
- GitHub organization: https://github.com/AppLovin

## APIs

This profile covers six AppLovin REST APIs across publisher (MAX) and advertiser (Growth / AppDiscovery / Axon) sides of the platform.

| API | Purpose | Base URL | Auth |
|---|---|---|---|
| [AppLovin MAX Revenue Reporting](https://support.axon.ai/en/max/reporting-apis/revenue-reporting-api) | Aggregated MAX mediation revenue and user-level publisher data | `https://r.applovin.com/maxReport` | `api_key` query (Report Key) |
| [AppLovin MAX Ad Unit Management](https://support.axon.ai/en/max/advanced-features/ad-unit-management-api) | Programmatic ad unit, waterfall, experiment, test-device management | `https://o.applovin.com/mediation/v1` | `Api-Key` header (Management Key) |
| [AppLovin Growth Reporting](https://support.axon.ai/en/growth/promoting-your-apps/api/reporting-api) | Campaign-level performance for AppDiscovery / Axon | `https://r.applovin.com/report` | `api_key` query (Report Key) |
| [AppLovin Growth Asset Reporting](https://support.axon.ai/en/growth/promoting-your-apps/api/asset-reporting-api) | Creative-level performance reporting | `https://r.applovin.com/assetReport` | `api_key` query (Report Key) |
| [AppLovin Axon Campaign Management](https://support.axon.ai/en/app-discovery/api/axon-campaign-management-api/) | Manage campaigns, creative sets, and creative assets | `https://api.ads.axon.ai/manage/v1` | `Authorization` header (Campaign Mgmt Key) |
| [AppLovin Conversion API for Lead Generation](https://support.axon.ai/en/growth/promoting-your-websites/api/lead-gen-capi/) | Server-to-server conversion event ingestion | `https://b.applovin.com/v1/event` | `Authorization` header (CAPI Key) |

## OpenAPI

Production-quality OpenAPI 3.0.3 specs were generated from the official AppLovin / Axon documentation:

- [openapi/applovin-max-revenue-reporting.yaml](openapi/applovin-max-revenue-reporting.yaml)
- [openapi/applovin-max-ad-unit-management.yaml](openapi/applovin-max-ad-unit-management.yaml)
- [openapi/applovin-growth-reporting.yaml](openapi/applovin-growth-reporting.yaml)
- [openapi/applovin-growth-asset-reporting.yaml](openapi/applovin-growth-asset-reporting.yaml)
- [openapi/applovin-axon-campaign-management.yaml](openapi/applovin-axon-campaign-management.yaml)
- [openapi/applovin-conversion-api-lead-gen.yaml](openapi/applovin-conversion-api-lead-gen.yaml)

Specs include `x-microcks-operation` extensions, `x-generated-from: documentation`, and `x-source-url` traceability.

## SDKs and Plugins

The [AppLovin GitHub organization](https://github.com/AppLovin) publishes the MAX mediation SDK across every major mobile and game-engine ecosystem:

| Platform | Repository |
|---|---|
| Android | [AppLovin-MAX-SDK-Android](https://github.com/AppLovin/AppLovin-MAX-SDK-Android) |
| iOS | [AppLovin-MAX-SDK-iOS](https://github.com/AppLovin/AppLovin-MAX-SDK-iOS) |
| Swift Package | [AppLovin-MAX-Swift-Package](https://github.com/AppLovin/AppLovin-MAX-Swift-Package) |
| Unity | [AppLovin-MAX-Unity-Plugin](https://github.com/AppLovin/AppLovin-MAX-Unity-Plugin) |
| React Native | [react-native-applovin-max](https://www.npmjs.com/package/react-native-applovin-max) |
| Flutter | [AppLovin-MAX-Flutter](https://github.com/AppLovin/AppLovin-MAX-Flutter) |
| Cordova | [AppLovin-MAX-Cordova](https://github.com/AppLovin/AppLovin-MAX-Cordova) |
| Unreal | [AppLovin-MAX-Unreal](https://github.com/AppLovin/AppLovin-MAX-Unreal) |
| Defold | [AppLovin-MAX-Defold](https://github.com/AppLovin/AppLovin-MAX-Defold) |
| Godot | [AppLovin-MAX-Godot](https://github.com/AppLovin/AppLovin-MAX-Godot) |
| Adobe AIR | [AppLovin-MAX-AIR](https://github.com/AppLovin/AppLovin-MAX-AIR) |
| Ad Review (Android) | [AppLovin-MAX-Ad-Review-SDK-Android](https://github.com/AppLovin/AppLovin-MAX-Ad-Review-SDK-Android) |
| Ad Review (iOS) | [AppLovin-MAX-Ad-Review-SDK-iOS](https://github.com/AppLovin/AppLovin-MAX-Ad-Review-SDK-iOS) |

Tooling: [CocoaPods-Specs](https://github.com/AppLovin/CocoaPods-Specs), [homebrew-Mobile-Tools](https://github.com/AppLovin/homebrew-Mobile-Tools).

## JSON Schema

- [json-schema/applovin-campaign-schema.json](json-schema/applovin-campaign-schema.json)
- [json-schema/applovin-creative-set-schema.json](json-schema/applovin-creative-set-schema.json)
- [json-schema/applovin-ad-unit-schema.json](json-schema/applovin-ad-unit-schema.json)
- [json-schema/applovin-conversion-event-schema.json](json-schema/applovin-conversion-event-schema.json)

## JSON Structure

- [json-structure/applovin-campaign-structure.json](json-structure/applovin-campaign-structure.json)
- [json-structure/applovin-ad-unit-structure.json](json-structure/applovin-ad-unit-structure.json)
- [json-structure/applovin-conversion-event-structure.json](json-structure/applovin-conversion-event-structure.json)

## JSON-LD

- [json-ld/applovin-context.jsonld](json-ld/applovin-context.jsonld) — JSON-LD context that aligns AppLovin terms to schema.org and an AppLovin-specific vocabulary namespace.

## Vocabulary

- [vocabulary/applovin-vocabulary.yml](vocabulary/applovin-vocabulary.yml) — domain vocabulary covering MAX, Axon, AppDiscovery, mediation, and conversion-tracking concepts.

## Examples

Operation examples for each API:

- [applovin-max-revenue-reporting-example.json](examples/applovin-max-revenue-reporting-example.json)
- [applovin-max-ad-unit-management-list-example.json](examples/applovin-max-ad-unit-management-list-example.json)
- [applovin-max-ad-unit-management-create-example.json](examples/applovin-max-ad-unit-management-create-example.json)
- [applovin-growth-reporting-example.json](examples/applovin-growth-reporting-example.json)
- [applovin-growth-asset-reporting-example.json](examples/applovin-growth-asset-reporting-example.json)
- [applovin-axon-campaign-management-create-example.json](examples/applovin-axon-campaign-management-create-example.json)
- [applovin-axon-campaign-management-list-example.json](examples/applovin-axon-campaign-management-list-example.json)
- [applovin-conversion-api-lead-gen-example.json](examples/applovin-conversion-api-lead-gen-example.json)

## Spectral Rules

[rules/applovin-rules.yml](rules/applovin-rules.yml) enforces the conventions observed across the AppLovin / Axon OpenAPI specs:

- HTTPS server URLs that resolve to `applovin.com` or `axon.ai`
- snake_case path segments and parameter names
- camelCase `operationId`, Title Case `summary` and tag names
- snake_case schema property names with explicit types
- Defined security schemes and 401 responses

## Naftiko Capabilities

Two workflow capabilities compose the six APIs into customer-facing roles:

- [capabilities/publisher-monetization.yaml](capabilities/publisher-monetization.yaml) — MAX mediation ops persona. Combines MAX Ad Unit Management + MAX Revenue Reporting.
- [capabilities/advertiser-growth.yaml](capabilities/advertiser-growth.yaml) — User acquisition / growth marketer persona. Combines Axon Campaign Management + CAPI + Growth Reporting + Asset Reporting.

Per-API consumed definitions live under [capabilities/shared/](capabilities/shared/).

Each workflow exposes a unified REST adapter (Spectral-compliant `/v1/...` paths) and an MCP adapter for AI-assisted automation.

## Maintainer

[API Evangelist](https://apievangelist.com)
