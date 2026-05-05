# applovin

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
