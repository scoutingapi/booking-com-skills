---
name: booking-com-full
description: "Complete Booking.com toolkit — search, availability, listing detail, price, cross-OTA price comparison and reviews, all in one unified schema. Install this when an agent needs broad Booking.com coverage. Powered by ScoutingAPI."
version: "1.0.0"
license: MIT-0
author: ScoutingAPI
homepage: https://scoutingapi.com
repository: https://github.com/scoutingapi/booking-com-skills
user-invocable: true
compatibility: Requires internet access to reach api.scoutingapi.com. No additional runtimes or dependencies needed.
required_environment_variables:
  - name: SCOUTINGAPI_KEY
    prompt: Your ScoutingAPI key (starts with scout_)
    help: Free key at https://scoutingapi.com/signup — no card. A scout_test_ sandbox key returns fixtures at zero cost.
    required_for: all API requests
tags: ["booking-com", "booking-com-api", "search", "availability", "reviews", "price-comparison", "travel", "accommodation"]
metadata: {"openclaw":{"emoji":"🧰","requires":{"env":["SCOUTINGAPI_KEY"]},"primaryEnv":"SCOUTINGAPI_KEY","homepage":"https://scoutingapi.com"},"hermes":{"tags":["booking-com","booking-com-api","search","availability","reviews","price-comparison","travel","accommodation"],"category":"travel"}}
---

# Booking.com — complete toolkit

The everything skill for Booking.com: search, availability, listing detail, price, cross-OTA price comparison and reviews — one key, one schema. Install the focused skills instead when you want a minimal tool surface.

## Setup

If `$SCOUTINGAPI_KEY` is not set, read [references/auth-setup.md](references/auth-setup.md) and follow it to get and store the key. A `scout_test_` sandbox key works for evaluation at zero cost.

## When to use this skill

**DO use when the user asks:**

- Any Booking.com data task — the agent picks the right call

**Do NOT use when:**

- You want the smallest possible tool surface — install a focused skill

## Required headers

Every request needs:

- **Authorization:** `Bearer $SCOUTINGAPI_KEY`
- **User-Agent:** your agent's name (e.g. `ClaudeCode/1.0`).

Base URL: `https://api.scoutingapi.com/v1`.

## Tools

### `GET /v1/search`

Discover properties matching a location, dates, occupancy and filters across one or more platforms. Results from every requested platform are normalized to the same Property shape and merged into a single, cursor-paginated list. This is the breadth / funnel endpoint — and the clearest demonstration of "one schema, every platform".

Key parameters:
- `location` — Place name ("Split, HR") or "lat,lng". Required.
- `checkIn` — YYYY-MM-DD; required if checkOut given; not in the past.
- `checkOut` — YYYY-MM-DD; required if checkIn given; must be after checkIn.
- `adults` — ≥ 1.
- `children` — ≥ 0.
- `childAges[]` — Length must equal children. Coarsened for Vrbo/Airbnb.

### `GET /v1/availability`

Get day-by-day availability for a known listing (or a batch of listings) on one platform over a date window. Each day reports whether it is available, its minimum-night requirement, and whether check-in / check-out / booking is allowed.

Key parameters:
- `platform` — Single platform (not a fan-out endpoint).
- `listingId` — A single listing id on platform.
- `listingIds[]` — A batch of listing ids on platform.
- `url` — Full listing URL (alternative to an id).
- `startDate` — YYYY-MM-DD; not in the past.
- `endDate` — After startDate; window ≤ 365 days.

### `GET /v1/listing/{platform}/{id}`

Full normalized detail for one listing: amenities (canonical taxonomy), photos, host, geo, ratings, and — when you pass dates — an embedded live price. The detail body is cached 24 h; any embedded live price is cached at the 1 h price TTL and composed at read time, so a stale price never rides on fresh detail.

Key parameters:
- `platform` — vrbo | booking | airbnb | google.
- `id` — Platform-native listing id (case-sensitive, verbatim from source).
- `checkIn` — Pairs with checkOut; presence embeds a best-effort live price (may be null; the call still bills).
- `checkOut` — Must be after checkIn.
- `adults` — ≥ 1 (only used with dates).
- `children` — ≥ 0.

### `GET /v1/price`

Quote one listing for specific dates and occupancy. Pass the platform-native platformListingId returned by /v1/search — numeric on Airbnb and Vrbo, a slug string on Booking.com and Google (e.g. "abramovic2") — or a full listing URL. The response is always a real numeric price or a typed error — never a wrong property's price.

Key parameters:
- `platform` — vrbo | booking | airbnb | google.
- `listingId` — Platform-native id from /v1/search platformListingId — numeric (Airbnb/Vrbo) or a slug string (Booking.com/Google). Or pass a url.
- `checkIn` — YYYY-MM-DD; not in the past.
- `checkOut` — Must be after checkIn.
- `adults` — ≥ 1.
- `children` — ≥ 0.

### `GET /v1/price-compare`

Compare the price of one property across booking platforms in a single call, resolved through the Google Hotels backbone. The response carries the offers plus ScoutingAPI-computed min and median as first-class fields, so you can read the cheapest and typical cross-OTA price without re-deriving them. No cross-platform short-term-rental price-comparison API exists elsewhere — this is the wedge.

Key parameters:
- `name` — Property name to resolve.
- `googleHotelId` — Precise Google Hotels id.
- `location` — Disambiguating place / "lat,lng".
- `checkIn` — YYYY-MM-DD; not in the past.
- `checkOut` — Must be after checkIn.
- `adults` — ≥ 1.

### `GET /v1/reviews`

Normalized, paginated reviews for one listing on one platform. Native rating scales are preserved and echoed alongside each rating (TripAdvisor/Airbnb/Vrbo use 5; Booking.com/Expedia/Hotels.com use 10) — never silently rescaled.

Key parameters:
- `platform` — Platform enum.
- `listingId` — Listing id on platform.
- `url` — Full listing URL.
- `limit` — 1–100.
- `cursor` — Opaque base64 cursor.
- `language` — ISO-639-1 filter.


> Filter results to Booking.com by passing `platforms=booking` to the search call.

## MCP (no key pasted into the agent)

On an MCP-capable runtime, connect `https://mcp.scoutingapi.com` (OAuth 2.1 + PKCE) and use: `search_stays`, `check_availability`, `get_listing`, `get_price`, `compare_prices`, `get_reviews`.

## The cross-OTA advantage

ScoutingAPI is **cross-platform**: Booking.com data comes back in the *same unified schema* as Airbnb, Booking.com, Vrbo and Google Hotels, and the price-compare tool returns a computed **min** and **median** across every OTA — something a single-platform wrapper can't do.

## Async & partial failures

A live call that has to scrape returns `202` + a `jobId`; poll `GET /v1/jobs/{jobId}` (free) until `data.status` is `completed`, then read `data.result`. On a fan-out, check `meta.partial` and `meta.platformResults[]`.

## Credits

Number-free by design — **failed, empty and blocked calls are never billed**, and `scout_test_` sandbox calls are always free. Current costs: <https://scoutingapi.com/pricing> · full contract: <https://api.scoutingapi.com/openapi.json>.

## Trademark

ScoutingAPI is an independent service and is not affiliated with, endorsed by, or sponsored by Booking.com. Booking.com is a trademark of its respective owner.

---

**Get your free key → https://scoutingapi.com/signup** · Docs: https://scoutingapi.com/docs
