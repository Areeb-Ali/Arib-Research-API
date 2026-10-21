# AriB Research · Event Calendar API

What scheduled events in the coming days and weeks could matter for crypto markets, and what just happened?

A small, free, keyed JSON API for the AriB Research Event Calendar. It lists dated, sourced events
(macro releases, crypto options and futures expiries, Ethereum upgrades, SEC/CFTC rulemaking deadlines, governance votes).
It never says how markets will react. Research only, not financial advice.

- **Docs page:** https://calendar.aribresearch.com/developers
- **Base URL:** `https://calendar.aribresearch.com/public-api/v1`
- **Methodology:** `1.0`

## Getting a key

Request a free key with the feedback form on the site (type: **Other**).
Keys are issued by hand, shown once, and expire after 365 days.
Send it in the `X-API-Key` header. Keys in the URL are refused.

```bash
curl -H "X-API-Key: YOUR_KEY" "https://calendar.aribresearch.com/public-api/v1/events?days=30&importance=high"
```

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute, per key | 30 |
| Requests per day, per key | 1000 |
| Requests per day, all keys together | 20000 |
| Rows per response | at most 500 |
| History | 30 days |
| Key lifetime | 365 days |

Over a limit you get HTTP `429` with `Retry-After`.
Every response carries `X-RateLimit-Limit`, `X-RateLimit-Reset`, `X-RateLimit-Limit-Day` and `X-RateLimit-Remaining-Day`.
There is no bulk export.

## Endpoints

| Endpoint | What it returns |
|----------|-----------------|
| `GET /public-api/v1/events` | Upcoming events in the next 7, 30 or 90 days, with filters |
| `GET /public-api/v1/events/recent` | Events whose scheduled time passed in the last days (short history) |
| `GET /public-api/v1/events/{id}` | One event with its change log (reschedules, cancellations) |
| `GET /public-api/v1/load` | Event load: count of high-importance events ahead, plus short daily history |
| `GET /public-api/v1/status` | Source status and coverage |
| `GET /public-api/v1/methodology` | Importance rules and categories (versioned) |

### Filters for `/events` and `/events/recent`

| Parameter | Values |
|-----------|--------|
| `days` | `/events`: 7, 30 or 90. `/events/recent`: 1 to 30 |
| `category` | `macro`, `derivatives`, `unlocks`, `etf`, `upgrades`, `governance`, `regulatory`, `other` |
| `importance` | `high`, `medium`, `low` |
| `asset` | Ticker such as `BTC`; market-wide events are always included |
| `origin` | `auto` or `manual` |
| `limit` | Rows to return (1 to 500) |

## Response format

```json
{"attribution": "...", "license": "...", "notice": "Research only, not financial advice", "methodology_version": "1.0",
 "as_of": "2026-10-21T12:00:00Z", "data": [...], "count": 12, "total": 12, "truncated": false}
```

Every event has:

- **precision:** `exact`, `date` (date only), `approximate` (computed from a rule, not yet confirmed)
- **status:** `scheduled`, `rescheduled`, `occurred`, `cancelled`
- **source** with a link
- **importance** with the rule that produced it

Importance is a published hypothesis about attention, not a prediction, and says nothing about direction.
Missing values are `null`; nothing is filled in.

Errors: `{"error": {"code": "...", "message": "...", "ref": "..."}}`

| Status | Meaning |
|--------|---------|
| 400 | Bad request |
| 401 | Missing or invalid key |
| 403 | Key expired or revoked |
| 429 | Limit reached; see `Retry-After` and `X-RateLimit-*` headers |

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `events` | Dated, sourced events: macro releases, crypto options and futures expiries, Ethereum network upgrades, SEC/CFTC rulemaking deadlines, governance votes, owner-curated events | event | checked every 30 minutes to 6 hours depending on the source |
| `events_recent` | Events whose scheduled time has passed (short history) | event | as events pass |
| `event_load` | Number of upcoming high-importance events in the next 7 and 30 days by category. Counts events; says nothing about market direction | count of events | computed on request; history one point per day |
| `source_status` | Freshness of every source and overall coverage | status | computed on request |
| `methodology` | Versioned importance rules and categories | rule | changes only with a new methodology version |

## License and attribution

- **License:** Personal and internal use. No redistribution or resale without written permission. Underlying US government schedules are in the public domain.
- **Attribution (required):** AriB Research Event Calendar. Event data from the Federal Reserve, BLS, BEA, Deribit, Binance, go-ethereum, the Federal Register and Snapshot.
- Research only, not financial advice.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
