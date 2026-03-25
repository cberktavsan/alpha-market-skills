---
name: commodity-tracker
description: Tracks energy, metals, and agricultural commodity prices via Alpha Vantage commodity endpoints.
metadata:
  version: "1.0.0"
---

# Commodity Tracker

The assistant tracks and analyzes commodity prices across energy, metals, and agriculture using Alpha Vantage.

> **Global rules (language, rate limits, free-tier constraints, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

### Energy

| Tool | Commodity |
|------|-----------|
| `WTI` | West Texas Intermediate crude oil (USD/barrel) |
| `BRENT` | Brent crude oil (USD/barrel) |
| `NATURAL_GAS` | Henry Hub natural gas (USD/MMBtu) |

### Metals

| Tool | Commodity |
|------|-----------|
| `COPPER` | Copper (USD/pound) |
| `ALUMINUM` | Aluminum (USD/ton) |
| `GOLD_SILVER_SPOT` | Current gold and silver spot prices |
| `GOLD_SILVER_HISTORY` | Historical gold and silver prices |

### Agriculture

| Tool | Commodity |
|------|-----------|
| `WHEAT` | Wheat (USD/bushel) |
| `CORN` | Corn (USD/bushel) |
| `COTTON` | Cotton (USD/pound) |
| `SUGAR` | Sugar (cents/pound) |
| `COFFEE` | Coffee (USD/pound) |

### Composite

| Tool | Purpose |
|------|---------|
| `ALL_COMMODITIES` | Global commodity index |

## Common Parameters

Most commodity endpoints accept:
- `interval`: `daily`, `weekly`, `monthly` (default: monthly)
- Data goes back 20+ years

## Workflows

### Current Commodity Prices

1. The assistant calls the specific commodity tool (e.g., `WTI`, `GOLD_SILVER_SPOT`)
2. Presents: current price, unit, date
3. For gold/silver, shows both metals side-by-side with gold/silver ratio

### Commodity Dashboard

1. The assistant calls multiple commodity endpoints:
   - Energy: `WTI`, `BRENT`, `NATURAL_GAS`
   - Metals: `GOLD_SILVER_SPOT`, `COPPER`
   - Agriculture: 2-3 relevant ones based on user context
2. Presents as a single table with: commodity, price, unit, recent trend direction
3. Limits to 5-6 calls to conserve rate limits

### Historical Commodity Analysis

1. The assistant calls the commodity endpoint with desired interval
2. Calculates: period change %, high/low range, average price
3. Identifies significant price moves and their approximate dates

### Energy Sector Overview

1. Calls `WTI`, `BRENT`, `NATURAL_GAS`
2. Shows WTI-Brent spread (premium/discount)
3. Presents trend context for each

### Precious Metals Analysis

1. Calls `GOLD_SILVER_SPOT` for current prices
2. Calls `GOLD_SILVER_HISTORY` with `interval=monthly` for trend
3. Calculates gold/silver ratio (historically ~60-80; extremes signal potential mean reversion)

## Gotchas

- **Default interval is monthly**: If the user asks for "current" or "today's" price, the assistant specifies `interval=daily` explicitly. The default monthly interval returns the latest monthly average, not today's price.
- **GOLD_SILVER_SPOT vs GOLD_SILVER_HISTORY**: SPOT returns current prices only (no time series). HISTORY returns historical data. The assistant does not call HISTORY when the user just wants today's gold price.
- **ALL_COMMODITIES returns an index, not individual prices**: This is a composite global commodity index value. The assistant uses it only for overall commodity market direction, not for individual commodity lookups.
- **Units vary by commodity**: WTI/Brent = USD/barrel, natural gas = USD/MMBtu, copper = USD/pound, aluminum = USD/ton, wheat/corn = USD/bushel, cotton = USD/pound, sugar = cents/pound (not dollars), coffee = USD/pound. The assistant always includes units.
- **Sugar is in cents**: Sugar prices are in cents per pound, not dollars. A reading of "22.5" means $0.225/pound. The assistant converts clearly for the user.
- **Weekend/holiday gaps in daily data**: Commodity markets have different trading hours than equities. Missing days are normal.

## Output Format

- The assistant always includes units (USD/barrel, USD/oz, USD/bushel, etc.)
- Shows daily/weekly/monthly change where data allows
- For energy: WTI and Brent side by side for context
- Notes data frequency and any delays

## Token Budget

Target: keep skill output under 400 tokens. For commodity dashboards, a single table with 5-6 commodities (price, unit, direction). No prose padding between rows.
