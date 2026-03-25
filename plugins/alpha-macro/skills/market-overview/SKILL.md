---
name: market-overview
description: Provides market snapshots with top gainers, losers, most active stocks, and global market status.
metadata:
  version: "1.0.0"
---

# Market Overview

The assistant provides broad market snapshots and identifies top movers using Alpha Vantage.

> **Global rules (language, rate limits, API limits, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Purpose |
|------|---------|
| `TOP_GAINERS_LOSERS` | Top 20 gainers, losers, and most actively traded US tickers |
| `MARKET_STATUS` | Open/closed status for all global exchanges |
| `GLOBAL_QUOTE` | Quick price check for market benchmarks |

## Workflows

### Daily Market Snapshot

1. The assistant calls `MARKET_STATUS` to show which markets are open/closed
2. Calls `TOP_GAINERS_LOSERS` for today's movers
3. Presents in three sections:
   - **Top Gainers**: Top 5 with ticker, price, change %, volume
   - **Top Losers**: Top 5 with ticker, price, change %, volume
   - **Most Active**: Top 5 by volume with ticker, price, change %
4. Adds brief commentary on market tone (are gainers dominating? is volume high?)

### Market Status Check

1. The assistant calls `MARKET_STATUS`
2. Presents a clean table: market name, region, status (open/closed), local time
3. Highlights major markets: NYSE, NASDAQ, London, Frankfurt, Tokyo, Hong Kong, Shanghai

### Benchmark Quick Check

1. The assistant calls `GLOBAL_QUOTE` for key ETFs the user cares about
2. Common benchmarks: SPY (S&P 500), QQQ (NASDAQ), DIA (Dow), IWM (Russell 2000)
3. Presents: price, daily change %, volume
4. Notes: these are ETFs tracking indices, not the indices themselves

## Gotchas

- **TOP_GAINERS_LOSERS returns 20 items per category**: The assistant presents only the top 5 in each category. Showing all 20 wastes tokens without adding value.
- **TOP_GAINERS_LOSERS is US-only**: This endpoint covers US equities only. If the user asks about non-US market movers, the assistant explains this limitation.
- **Penny stocks dominate the gainers list**: The top gainers often include micro-cap or penny stocks with extreme percentage moves. The assistant notes this context so the user does not mistake it for a major stock rally.
- **SPY/QQQ/DIA are ETFs, not indices**: When using GLOBAL_QUOTE for market benchmarks, the assistant clarifies "SPY (S&P 500 ETF proxy)" rather than saying "S&P 500 is at X."
- **MARKET_STATUS returns all global exchanges**: There are 20+ exchanges in the response. The assistant presents only the ones relevant to the user's context. Default to: NYSE, NASDAQ, London, Frankfurt, Tokyo, Hong Kong.
- **Market status timezone**: MARKET_STATUS returns UTC-based times. The assistant converts to the user's likely timezone when presenting.

## Output Format

The assistant keeps market overviews concise and scannable:
- Leads with the most important info (market direction, biggest movers)
- Uses tables for top gainers/losers
- Includes timestamps
- Notes if data is delayed

## Token Budget

Target: keep skill output under 400 tokens. For daily snapshots, a brief market status line followed by three 5-row tables (gainers, losers, active).
