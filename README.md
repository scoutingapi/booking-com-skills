<!-- generated: DO NOT EDIT BY HAND — produced by apps/web/scripts/export-github-repos.ts from lib/agent/skills machinery + the endpoint SSOT. Edit the config + re-run. -->

# Booking.com Skills for AI Agents

**Agent skills for Booking.com data** — search, availability, prices, cross-OTA price comparison and reviews, from any AI agent, over the [ScoutingAPI](https://scoutingapi.com) REST API and hosted MCP server.

Works with **OpenClaw** (ClawdBot/Moltbot), **Hermes Agent**, **Claude Code**, **Cursor**, **Cline**, **Codex**, and any agent that supports the [Agent Skills](https://skills.sh) format. Free tier, no credit card, and a `scout_test_` sandbox that returns fixtures at zero cost.

## Install

Most users want **booking-com-full** — it covers search, availability, listing, price, cross-OTA comparison and reviews in one skill.

**OpenClaw (ClawdBot/Moltbot), via ClawHub:**
```bash
npx clawhub@latest install booking-com-full
```

**Hermes Agent:**
```bash
hermes skills install skills-sh/scoutingapi/booking-com-skills/skills/booking-com-full
```

**Claude Code / Cursor / Cline / Codex:**
```bash
npx skills add scoutingapi/booking-com-skills --skill booking-com-full
```

**All skills at once:**
```bash
npx skills add scoutingapi/booking-com-skills
```

**Manual (git clone):**
```bash
git clone https://github.com/scoutingapi/booking-com-skills.git
cp -r booking-com-skills/skills/booking-com-full ~/.claude/skills/
```

> The `npx skills add scoutingapi/booking-com-skills` route reads this GitHub repo directly and works today. ClawHub / Hermes directory installs resolve once the skills are indexed in those directories.

## Skills in this repo

| Skill | Purpose |
|---|---|
| [`booking-com-search`](skills/booking-com-search) | Booking.com search |
| [`booking-com-availability`](skills/booking-com-availability) | Booking.com availability |
| [`booking-com-reviews`](skills/booking-com-reviews) | Booking.com reviews |
| [`booking-com-prices`](skills/booking-com-prices) | Booking.com prices & cross-OTA comparison |
| [`booking-com-full`](skills/booking-com-full) | Booking.com — complete toolkit |
| [`booking-com-hotels`](skills/booking-com-hotels) | Booking.com hotels |

Install `booking-com-full` for broad coverage; install a focused skill when you want a minimal tool surface.

## Authentication

Set `SCOUTINGAPI_KEY` to your ScoutingAPI key. Free key (no card) at <https://scoutingapi.com/signup>; a `scout_test_` sandbox key returns deterministic fixtures at zero cost. See [`skills/booking-com-full/references/auth-setup.md`](skills/booking-com-full/references/auth-setup.md).

## Why ScoutingAPI

One integration covers every platform: Booking.com data comes back in the same unified schema as Airbnb, Booking.com, Vrbo and Google Hotels, and the price-compare tool returns a computed min and median across OTAs. Number-free on credits — failed/empty calls are never billed; costs are on the [pricing page](https://scoutingapi.com/pricing).

## Source

- API reference: <https://api.scoutingapi.com/openapi.json>
- Hosted MCP server: <https://mcp.scoutingapi.com/mcp>
- Docs: <https://scoutingapi.com/docs>

---

## Related repositories

Part of the ScoutingAPI open resource set — one unified accommodation-data API across Airbnb, Booking.com, Vrbo and Google Hotels, with a REST API, a native MCP server, and importable workflows.

- [airbnb-api](https://github.com/scoutingapi/airbnb-api)
- [booking-com-api](https://github.com/scoutingapi/booking-com-api)
- [vrbo-api](https://github.com/scoutingapi/vrbo-api)
- [google-hotels-api](https://github.com/scoutingapi/google-hotels-api)
- [hotel-api](https://github.com/scoutingapi/hotel-api)
- [travel-api](https://github.com/scoutingapi/travel-api)
- [travel-skills](https://github.com/scoutingapi/travel-skills)
- [hotel-mcp](https://github.com/scoutingapi/hotel-mcp)
- [travel-workflows](https://github.com/scoutingapi/travel-workflows)
- [airbnb-skills](https://github.com/scoutingapi/airbnb-skills)
- [vrbo-skills](https://github.com/scoutingapi/vrbo-skills)
- [google-hotels-skills](https://github.com/scoutingapi/google-hotels-skills)

- Website: <https://scoutingapi.com> · Docs: <https://scoutingapi.com/docs> · Status: <https://status.scoutingapi.com>

---

**Get your free key — no card → <https://scoutingapi.com/signup>** · [Docs](https://scoutingapi.com/docs) · [Pricing](https://scoutingapi.com/pricing) · [Status](https://status.scoutingapi.com)

Built with [ScoutingAPI](https://scoutingapi.com) — trusted, ToS-clean, real-time accommodation data across Airbnb, Booking.com, Vrbo and Google Hotels, normalized to one schema. REST + a native MCP server.
> ScoutingAPI is an independent service and is not affiliated with or endorsed by Booking.com. Booking.com is a trademark of its respective owner.
