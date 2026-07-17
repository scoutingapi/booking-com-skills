---
name: booking-com-hotels
description: "Search Booking.com hotels by location, dates and occupancy in one unified schema, with cross-OTA price comparison. Use for hotel discovery on Booking.com. Powered by ScoutingAPI."
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
tags: ["booking-com", "hotels", "hotel-search", "price-comparison", "travel"]
metadata: {"openclaw":{"emoji":"🏨","requires":{"env":["SCOUTINGAPI_KEY"]},"primaryEnv":"SCOUTINGAPI_KEY","homepage":"https://scoutingapi.com"},"hermes":{"tags":["booking-com","hotels","hotel-search","price-comparison","travel"],"category":"travel"}}
---

# Booking.com hotels

Hotel discovery on Booking.com — search live hotel stays in the unified schema, with cross-OTA price comparison built in.

## Setup

If `$SCOUTINGAPI_KEY` is not set, read [references/auth-setup.md](references/auth-setup.md) and follow it to get and store the key. A `scout_test_` sandbox key works for evaluation at zero cost.

## When to use this skill

**DO use when the user asks:**

- "Find Booking.com hotels in Zadar for two nights"
- "Cheapest Booking.com hotel near the old town"

**Do NOT use when:**

- You have one known hotel and just want its price — use booking-com-prices

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

### `GET /v1/price-compare`

Compare the price of one property across booking platforms in a single call, resolved through the Google Hotels backbone. The response carries the offers plus ScoutingAPI-computed min and median as first-class fields, so you can read the cheapest and typical cross-OTA price without re-deriving them. No cross-platform short-term-rental price-comparison API exists elsewhere — this is the wedge.

Key parameters:
- `name` — Property name to resolve.
- `googleHotelId` — Precise Google Hotels id.
- `location` — Disambiguating place / "lat,lng".
- `checkIn` — YYYY-MM-DD; not in the past.
- `checkOut` — Must be after checkIn.
- `adults` — ≥ 1.


> Filter results to Booking.com by passing `platforms=booking` to the search call.

## MCP (no key pasted into the agent)

On an MCP-capable runtime, connect `https://mcp.scoutingapi.com/mcp` (OAuth 2.1 + PKCE) and use: `search_stays`, `compare_prices`.

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
