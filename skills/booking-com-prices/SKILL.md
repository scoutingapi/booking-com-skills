---
name: booking-com-prices
description: "Get a real Booking.com price quote for a listing and dates, and compare that property across booking platforms for the cheapest rate. Use for \"how much is this Booking.com place\" or \"is it cheaper elsewhere\". Powered by ScoutingAPI."
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
tags: ["booking-com", "booking-com-api", "price", "price-comparison", "cross-ota", "travel", "accommodation"]
metadata: {"openclaw":{"emoji":"💲","requires":{"env":["SCOUTINGAPI_KEY"]},"primaryEnv":"SCOUTINGAPI_KEY","homepage":"https://scoutingapi.com"},"hermes":{"tags":["booking-com","booking-com-api","price","price-comparison","cross-ota","travel","accommodation"],"category":"travel"}}
---

# Booking.com prices & cross-OTA comparison

Get a real Booking.com price quote — and, because ScoutingAPI is cross-platform, compare the same property across OTAs for the cheapest rate with a computed min and median.

## Setup

If `$SCOUTINGAPI_KEY` is not set, read [references/auth-setup.md](references/auth-setup.md) and follow it to get and store the key. A `scout_test_` sandbox key works for evaluation at zero cost.

## When to use this skill

**DO use when the user asks:**

- "How much is this Booking.com listing for these dates?"
- "Is this Booking.com place cheaper on another site?"

**Do NOT use when:**

- The user wants a list of options — use the search skill

## Required headers

Every request needs:

- **Authorization:** `Bearer $SCOUTINGAPI_KEY`
- **User-Agent:** your agent's name (e.g. `ClaudeCode/1.0`).

Base URL: `https://api.scoutingapi.com/v1`.

## Tools

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


## MCP (no key pasted into the agent)

On an MCP-capable runtime, connect `https://mcp.scoutingapi.com` (OAuth 2.1 + PKCE) and use: `get_price`, `compare_prices`.

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
