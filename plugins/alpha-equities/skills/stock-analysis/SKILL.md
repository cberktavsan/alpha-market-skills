---
name: stock-analysis
description: Retrieves stock prices, quotes, historical data, and symbol lookups via Alpha Vantage equity endpoints.
metadata:
  version: "1.0.0"
---

# Stock Analysis

The assistant provides stock price data, quotes, and equity market analysis using Alpha Vantage MCP tools.

> **Global rules (language, rate limits, API limits, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Purpose |
|------|---------|
| `GLOBAL_QUOTE` | Latest price, change, volume for a single ticker |
| `TIME_SERIES_DAILY` | Raw daily OHLCV data (up to 20+ years) |
| `TIME_SERIES_WEEKLY` | Weekly aggregated OHLCV |
| `TIME_SERIES_WEEKLY_ADJUSTED` | Weekly with adjusted close + dividends |
| `TIME_SERIES_MONTHLY` | Monthly aggregated OHLCV |
| `TIME_SERIES_MONTHLY_ADJUSTED` | Monthly with adjusted close + dividends |
| `SYMBOL_SEARCH` | Find ticker symbols by keyword |
| `MARKET_STATUS` | Global market open/close status |
| `TIME_SERIES_INTRADAY` | Intraday OHLCV at 1/5/15/30/60 min intervals *(premium)* |
| `TIME_SERIES_DAILY_ADJUSTED` | Daily OHLCV adjusted for splits and dividends *(premium)* |
| `REALTIME_BULK_QUOTES` | Batch real-time quotes for up to 100 symbols *(premium)* |
| `REALTIME_OPTIONS` | Real-time options chain data *(premium)* |
| `HISTORICAL_OPTIONS` | Historical options chain data *(premium)* |

## Workflows

### Single Stock Price Check

1. The assistant calls `GLOBAL_QUOTE` with the ticker symbol
2. Presents: current price, change ($), change (%), volume, previous close, 52-week high/low if available
3. Adds brief context (up/down today, significance of the move)

### Historical Price Analysis

1. The assistant determines the appropriate time series based on the user's timeframe:
   - Last few weeks → `TIME_SERIES_DAILY` with `outputsize=compact` (last 100 data points)
   - Last few months → `TIME_SERIES_WEEKLY`
   - Last few years → `TIME_SERIES_MONTHLY`
2. Calculates key metrics from the data: price range, percentage change over period, average volume
3. Identifies notable patterns: new highs/lows, trend direction, volume spikes

### Stock Comparison

1. The assistant calls `GLOBAL_QUOTE` for each ticker (max 3-4 to respect rate limits)
2. Presents a comparison table with: price, daily change %, volume, market cap if available
3. Highlights which is outperforming

### Symbol Search

1. The assistant calls `SYMBOL_SEARCH` with the user's keywords
2. Presents top matches with: symbol, name, type, region, currency
3. Lets the user pick which one to analyze further

### Market Status Check

1. The assistant calls `MARKET_STATUS`
2. Presents which major markets are open/closed with timezone context

## Gotchas

- **GLOBAL_QUOTE returns empty on weekends/holidays**: If all fields are "0.0000" or missing, the market is closed. The assistant states this clearly rather than presenting zero values.
- **TIME_SERIES_DAILY default is compact (100 points)**: For longer history, pass `outputsize=full`, but this returns 20+ years of data (~5000 rows). The assistant always uses `compact` unless the user explicitly requests full history.
- **SYMBOL_SEARCH is fuzzy**: Searching "Apple" returns AAPL but also unrelated matches. The assistant confirms with the user if the match is ambiguous.
- **Volume = 0 on some exchanges**: Non-US tickers sometimes return 0 volume. This does not mean no trading occurred; the data source may not carry volume for that exchange.
- **"Information" key in response**: If the API returns a JSON object with an "Information" key instead of data, the daily limit has been hit. The assistant does not retry; it informs the user.
- **Adjusted vs unadjusted**: TIME_SERIES_DAILY is unadjusted. For splits/dividends history, the assistant uses WEEKLY_ADJUSTED or MONTHLY_ADJUSTED instead.

## Output Format

The assistant presents data in clean, structured format:
- Tables for comparisons
- Percentage changes with directional indicators
- Data timestamp/date always stated
- For TIME_SERIES data, only the 5-10 most recent data points in a table; the rest summarized as "over the past N days, the stock moved from X to Y (Z%)"
- For GLOBAL_QUOTE, a compact block (not a table)

## Token Budget

Target: keep skill output under 500 tokens per workflow. For TIME_SERIES_DAILY responses, the assistant extracts the 5 most recent trading days only unless the user requests a specific range.
