---
name: booking-com-reviews
description: "Read normalized Booking.com reviews for a listing, with native rating scales preserved. Use when a user wants ratings or guest feedback for a listing on Booking.com. Powered by ScoutingAPI."
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
tags: ["booking-com", "booking-com-api", "reviews", "ratings", "travel", "accommodation"]
metadata: {"openclaw":{"emoji":"⭐","requires":{"env":["SCOUTINGAPI_KEY"]},"primaryEnv":"SCOUTINGAPI_KEY","homepage":"https://scoutingapi.com"},"hermes":{"tags":["booking-com","booking-com-api","reviews","ratings","travel","accommodation"],"category":"travel"}}
---

# Booking.com reviews

Read normalized Booking.com reviews for a listing — the same review shape across every platform.

## Setup

If `$SCOUTINGAPI_KEY` is not set, read [references/auth-setup.md](references/auth-setup.md) and follow it to get and store the key. A `scout_test_` sandbox key works for evaluation at zero cost.

## When to use this skill

**DO use when the user asks:**

- "What do reviews say about this Booking.com place?"
- "Pull the latest Booking.com guest ratings"

**Do NOT use when:**

- You do not have a listing id — search first

## Required headers

Every request needs:

- **Authorization:** `Bearer $SCOUTINGAPI_KEY`
- **User-Agent:** your agent's name (e.g. `ClaudeCode/1.0`).

Base URL: `https://api.scoutingapi.com/v1`.

## Tools

### `GET /v1/reviews`

Normalized, paginated reviews for one listing on one platform. Native rating scales are preserved and echoed alongside each rating (TripAdvisor/Airbnb/Vrbo use 5; Booking.com/Expedia/Hotels.com use 10) — never silently rescaled.

Key parameters:
- `platform` — Platform enum.
- `listingId` — Listing id on platform.
- `url` — Full listing URL.
- `limit` — 1–100.
- `cursor` — Opaque base64 cursor.
- `language` — ISO-639-1 filter.


## MCP (no key pasted into the agent)

On an MCP-capable runtime, connect `https://mcp.scoutingapi.com` (OAuth 2.1 + PKCE) and use: `get_reviews`.

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
