---
name: forex-analysis
description: Analyzes forex currency pairs with real-time rates and historical OHLC data via Alpha Vantage FX endpoints.
metadata:
  version: "1.0.0"
---

# Forex Analysis

The assistant analyzes currency pairs and exchange rates using Alpha Vantage forex endpoints.

> **Global rules (language, rate limits, free-tier constraints, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Purpose |
|------|---------|
| `CURRENCY_EXCHANGE_RATE` | Real-time exchange rate between any two currencies |
| `FX_DAILY` | Daily OHLC for a currency pair |
| `FX_WEEKLY` | Weekly OHLC for a currency pair |
| `FX_MONTHLY` | Monthly OHLC for a currency pair |

## Currency Codes

Common codes: USD, EUR, GBP, JPY, TRY, CHF, CAD, AUD, NZD, CNY, KRW, INR, BRL, MXN, ZAR, SGD, HKD, SEK, NOK, DKK, PLN, CZK, HUF, RUB

## Workflows

### Current Exchange Rate

1. The assistant calls `CURRENCY_EXCHANGE_RATE` with `from_currency` and `to_currency`
2. Presents: rate, bid, ask, timestamp
3. Calculates inverse rate automatically (if user asks EUR/USD, also shows USD/EUR)

### Historical Forex Analysis

1. The assistant determines the timeframe:
   - Short-term (days/weeks) → `FX_DAILY`
   - Medium-term (months) → `FX_WEEKLY`
   - Long-term (years) → `FX_MONTHLY`
2. Calculates: period high, low, average, percentage change, range
3. Identifies trend direction and notable levels

### Multi-Currency Comparison

1. The assistant calls `CURRENCY_EXCHANGE_RATE` for each pair (e.g., USD/EUR, USD/GBP, USD/TRY)
2. Presents a cross-rate table
3. Highlights strongest and weakest currencies

### Forex + Technical Analysis

1. The assistant gets price data via `FX_DAILY` or `FX_WEEKLY`
2. Calls technical indicator tools (RSI, MACD, BBANDS) directly via the Alpha Vantage MCP server
3. Common forex indicators: RSI (14), MACD, Bollinger Bands, SMA (20, 50, 200)

## Common User Queries

- "How much is the dollar?" → The assistant calls CURRENCY_EXCHANGE_RATE with from=USD, to=user's local currency
- "EUR to USD?" → CURRENCY_EXCHANGE_RATE with from=EUR, to=USD
- "GBP rate?" → CURRENCY_EXCHANGE_RATE with from=GBP, to=USD

## Gotchas

- **CURRENCY_EXCHANGE_RATE returns a single snapshot, not a time series**: For historical data, the assistant uses FX_DAILY/WEEKLY/MONTHLY instead.
- **FX_DAILY returns 100+ rows by default**: The assistant extracts only the most recent 5-10 trading days unless the user specifies a longer range.
- **Bid/Ask spread matters for exotic pairs**: For major pairs (EUR/USD, GBP/USD) the spread is tight. For exotic pairs (USD/TRY, USD/ZAR) the spread can be significant. The assistant notes the spread when it exceeds 0.1%.
- **"Dollar rate" is ambiguous**: The assistant infers the counter currency from the user's language. Turkish-language user asking about "dolar" likely means USD/TRY. English-language user may mean a specific pair.
- **Always show inverse rate**: If the user asks for EUR/USD, the assistant also calculates and displays USD/EUR = 1/rate.
- **Decimal precision varies**: Major pairs use 4-6 decimal places. JPY pairs use 2-3. Emerging market pairs use 2-4. The assistant matches market convention.
- **Weekend rates**: FX markets close Friday 5PM EST to Sunday 5PM EST. Weekend queries return Friday's close.

## Output Format

- The assistant always shows both directions (EUR→USD and USD→EUR)
- Includes timestamp and notes if delayed
- For historical data, shows clear date ranges
- Uses appropriate decimal precision by pair type

## Token Budget

Target: keep skill output under 400 tokens. For single exchange rate queries, a compact 3-4 line block. For historical analysis, a 5-row table plus a 2-sentence trend summary.
