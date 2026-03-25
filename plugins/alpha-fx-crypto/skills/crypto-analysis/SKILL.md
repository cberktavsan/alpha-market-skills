---
name: crypto-analysis
description: Analyzes cryptocurrency prices and trends with real-time rates and historical data via Alpha Vantage crypto endpoints.
metadata:
  version: "1.0.0"
---

# Crypto Analysis

The assistant analyzes cryptocurrency prices and trends using Alpha Vantage crypto endpoints.

> **Global rules (language, rate limits, API limits, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Purpose |
|------|---------|
| `CURRENCY_EXCHANGE_RATE` | Real-time exchange rate for any crypto-to-fiat or crypto-to-crypto pair |
| `DIGITAL_CURRENCY_DAILY` | Daily OHLCV in both crypto and fiat terms |
| `DIGITAL_CURRENCY_WEEKLY` | Weekly OHLCV |
| `DIGITAL_CURRENCY_MONTHLY` | Monthly OHLCV |

## Common Crypto Symbols

BTC, ETH, SOL, BNB, XRP, ADA, DOGE, AVAX, DOT, MATIC, LINK, UNI, ATOM, LTC, NEAR, APT, ARB, OP, FIL, AAVE

## Workflows

### Current Crypto Price

1. The assistant calls `CURRENCY_EXCHANGE_RATE` with `from_currency=BTC` (or any crypto) and `to_currency=USD` (or user's preferred fiat)
2. Presents: rate, bid, ask, timestamp
3. Also shows in the user's local currency if their context suggests it

### Historical Crypto Analysis

1. The assistant chooses the timeframe:
   - Days/weeks → `DIGITAL_CURRENCY_DAILY`
   - Months → `DIGITAL_CURRENCY_WEEKLY`
   - Years → `DIGITAL_CURRENCY_MONTHLY`
2. Calculates: period return %, high/low range, average
3. Notes: Alpha Vantage crypto data includes both USD and market-specific values

### Multi-Crypto Comparison

1. The assistant calls `CURRENCY_EXCHANGE_RATE` for each crypto (e.g., BTC/USD, ETH/USD, SOL/USD)
2. Presents comparison table with current prices
3. For historical comparison, uses daily/weekly data to show relative performance

### Crypto + Technical Analysis

1. The assistant gets price data via `DIGITAL_CURRENCY_DAILY`
2. Calls technical indicator tools (RSI, MACD, BBANDS) directly via the Alpha Vantage MCP server
3. Popular crypto indicators: RSI (14), MACD, Bollinger Bands, SMA 50/200
4. Uses the base symbol format (e.g., `BTC` not `BTCUSD`)

## Gotchas

- **from_currency uses base symbol only**: The assistant uses `from_currency=BTC` and `to_currency=USD`, not `from_currency=BTCUSD`. The `to_currency` parameter specifies the quote currency separately.
- **DIGITAL_CURRENCY_DAILY returns dual columns**: Each day has values in both the "market" (exchange-specific) and USD terms. The assistant uses USD columns for consistency unless the user requests a specific fiat.
- **Crypto symbols are base symbols only**: `BTC`, `ETH`, `SOL` — not `BTCUSD` or `BTC-USD`.
- **24/7 market**: Unlike stocks, crypto never closes. The assistant always includes the exact timestamp, not just the date.
- **High volatility calibration**: Crypto moves of 5-10% in a day are common, unlike equities where 2-3% is notable. The assistant calibrates language accordingly ("moderate move" for crypto differs from equities).
- **Small altcoin data gaps**: Some altcoins may have missing days or thin data. If the response is sparse, the assistant notes the data limitation.
- **Stablecoin queries**: If the user asks about USDT or USDC, the price will be approximately $1.00. The assistant provides the slight deviation as the meaningful data point (e.g., "USDT at $0.9998, within normal peg range").

## Output Format

- The assistant shows prices in USD by default, adds local currency if user context suggests
- Includes 24h change context when available
- Uses appropriate decimal precision (BTC: 2 decimals, small altcoins: more)
- Always notes timestamps — crypto markets are 24/7

## Token Budget

Target: keep skill output under 400 tokens. For a single crypto price, a compact 3-4 line block. For historical analysis, a 5-row table plus a 2-sentence trend summary.
