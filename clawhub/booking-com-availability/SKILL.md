---
name: booking-com-availability
description: "Check day-by-day Booking.com availability (the booking calendar) for a known listing over a date window. Use when a user asks whether a listing on Booking.com is open on specific dates. Powered by ScoutingAPI."
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
tags: ["booking-com", "booking-com-api", "availability", "calendar", "travel", "accommodation"]
metadata: {"openclaw":{"emoji":"📅","requires":{"env":["SCOUTINGAPI_KEY"]},"primaryEnv":"SCOUTINGAPI_KEY","homepage":"https://scoutingapi.com"},"hermes":{"tags":["booking-com","booking-com-api","availability","calendar","travel","accommodation"],"category":"travel"}}
---

# Booking.com availability

Check day-by-day availability for a known Booking.com listing — the booking calendar over any date window.

## Setup

If `$SCOUTINGAPI_KEY` is not set, read [references/auth-setup.md](references/auth-setup.md) and follow it to get and store the key. A `scout_test_` sandbox key works for evaluation at zero cost.

## When to use this skill

**DO use when the user asks:**

- "Is this Booking.com listing free the first week of August?"
- "Show the calendar for this Booking.com place"

**Do NOT use when:**

- You do not yet have a listing id — search first
- The user wants a price, not open/closed dates — use the prices skill

## Required headers

Every request needs:

- **Authorization:** `Bearer $SCOUTINGAPI_KEY`
- **User-Agent:** your agent's name (e.g. `ClaudeCode/1.0`).

Base URL: `https://api.scoutingapi.com/v1`.

## Tools

### `GET /v1/availability`

Get day-by-day availability for a known listing (or a batch of listings) on one platform over a date window. Each day reports whether it is available, its minimum-night requirement, and whether check-in / check-out / booking is allowed.

Key parameters:
- `platform` — Single platform (not a fan-out endpoint).
- `listingId` — A single listing id on platform.
- `listingIds[]` — A batch of listing ids on platform.
- `url` — Full listing URL (alternative to an id).
- `startDate` — YYYY-MM-DD; not in the past.
- `endDate` — After startDate; window ≤ 365 days.


## MCP (no key pasted into the agent)

On an MCP-capable runtime, connect `https://mcp.scoutingapi.com` (OAuth 2.1 + PKCE) and use: `check_availability`.

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
