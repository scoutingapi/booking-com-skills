<!-- generated: DO NOT EDIT BY HAND — produced by apps/web/scripts/export-github-repos.ts from lib/agent/skills machinery + the endpoint SSOT. Edit the config + re-run. -->

# Booking.com Skills for AI Agents

**Agent skills for Booking.com data** — search, availability, prices, cross-OTA price comparison and reviews, from any AI agent, over the [StayingAPI](https://stayingapi.com) REST API and hosted MCP server.

Works with **OpenClaw** (ClawdBot/Moltbot), **Hermes Agent**, **Claude Code**, **Cursor**, **Cline**, **Codex**, and any agent that supports the [Agent Skills](https://skills.sh) format. Free tier, no credit card, and a `stay_test_` sandbox that returns fixtures at zero cost.

## Install

Most users want **booking-com-full** — it covers search, availability, listing, price, cross-OTA comparison and reviews in one skill.

**OpenClaw (ClawdBot/Moltbot), via ClawHub:**
```bash
npx clawhub@latest install @stayingapi/booking-com-full
```

**Hermes Agent:**
```bash
hermes skills install skills-sh/stayingapi/booking-com-skills/skills/booking-com-full
```

**Claude Code / Cursor / Cline / Codex:**
```bash
npx skills add stayingapi/booking-com-skills --skill booking-com-full
```

**All skills at once:**
```bash
npx skills add stayingapi/booking-com-skills
```

**Manual (git clone):**
```bash
git clone https://github.com/stayingapi/booking-com-skills.git
cp -r booking-com-skills/skills/booking-com-full ~/.claude/skills/
```

> The `npx skills add stayingapi/booking-com-skills` route reads this GitHub repo directly and works today. The ClawHub listings are published and pending directory review — that command works as soon as they are released. Hermes installs resolve once the skills are indexed there. ClawHub slugs are not globally unique, so always install ours owner-scoped (`@stayingapi/…`).

## Skills in this repo

| Skill | Purpose |
|---|---|
| [`booking-com-search`](skills/booking-com-search) | Booking.com Search |
| [`booking-com-availability`](skills/booking-com-availability) | Booking.com Availability |
| [`booking-com-reviews`](skills/booking-com-reviews) | Booking.com Reviews |
| [`booking-com-prices`](skills/booking-com-prices) | Booking.com Prices & Cross-OTA Comparison |
| [`booking-com-full`](skills/booking-com-full) | Booking.com, Complete Toolkit |
| [`booking-com-hotels`](skills/booking-com-hotels) | Booking.com Hotels |

Install `booking-com-full` for broad coverage; install a focused skill when you want a minimal tool surface.

## Authentication

Set `STAYINGAPI_KEY` to your StayingAPI key. Free key (no card) at <https://stayingapi.com/signup>; a `stay_test_` sandbox key returns deterministic fixtures at zero cost. See [`skills/booking-com-full/references/auth-setup.md`](skills/booking-com-full/references/auth-setup.md).

## Why StayingAPI

One integration covers every platform: Booking.com data comes back in the same unified schema as Airbnb, Booking.com, Vrbo and Google Hotels, and the price-compare tool returns a computed min and median across OTAs. Number-free on credits — failed/empty calls are never billed; costs are on the [pricing page](https://stayingapi.com/pricing).

## Source

- API reference: <https://api.stayingapi.com/openapi.json>
- Hosted MCP server: <https://mcp.stayingapi.com/mcp>
- Docs: <https://stayingapi.com/docs>

---

## Related repositories

Part of the StayingAPI open resource set — one unified accommodation-data API across Airbnb, Booking.com, Vrbo and Google Hotels, with a REST API, a native MCP server, and importable workflows.

- [airbnb-api](https://github.com/stayingapi/airbnb-api)
- [booking-com-api](https://github.com/stayingapi/booking-com-api)
- [vrbo-api](https://github.com/stayingapi/vrbo-api)
- [google-hotels-api](https://github.com/stayingapi/google-hotels-api)
- [hotel-api](https://github.com/stayingapi/hotel-api)
- [travel-api](https://github.com/stayingapi/travel-api)
- [travel-skills](https://github.com/stayingapi/travel-skills)
- [hotel-mcp](https://github.com/stayingapi/hotel-mcp)
- [travel-workflows](https://github.com/stayingapi/travel-workflows)
- [airbnb-skills](https://github.com/stayingapi/airbnb-skills)
- [vrbo-skills](https://github.com/stayingapi/vrbo-skills)
- [google-hotels-skills](https://github.com/stayingapi/google-hotels-skills)

- Website: <https://stayingapi.com> · Docs: <https://stayingapi.com/docs> · Status: <https://status.stayingapi.com>

---

**Get your free key — no card → <https://stayingapi.com/signup>** · [Docs](https://stayingapi.com/docs) · [Pricing](https://stayingapi.com/pricing) · [Status](https://status.stayingapi.com)

Built with [StayingAPI](https://stayingapi.com) — trusted, ToS-clean, real-time accommodation data across Airbnb, Booking.com, Vrbo and Google Hotels, normalized to one schema. REST + a native MCP server.
> StayingAPI is an independent service and is not affiliated with or endorsed by Booking.com. Booking.com is a trademark of its respective owner.
