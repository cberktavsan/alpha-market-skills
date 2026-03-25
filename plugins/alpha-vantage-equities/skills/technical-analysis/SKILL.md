---
name: technical-analysis
description: Computes and interprets 40+ technical indicators for stocks, forex, and crypto via Alpha Vantage.
metadata:
  version: "1.0.0"
---

# Technical Analysis

The assistant performs comprehensive technical analysis using 40+ indicators from Alpha Vantage MCP.

> **Global rules (language, rate limits, free-tier constraints, output handling) are defined in this plugin's orchestrator and apply here.**

## Indicator Tiers

### Primary (use first — cover 90% of requests)

| Indicator | Category | Default Period | Key Signal |
|---|---|---|---|
| `RSI` | Momentum | 14 | >70 overbought, <30 oversold |
| `MACD` | Trend | 12/26/9 | Signal line crossover |
| `BBANDS` | Volatility | 20, 2 stddev | Squeeze = breakout coming |
| `SMA` | Trend | 20/50/200 | Golden/Death Cross |
| `EMA` | Trend | 20/50/200 | Faster than SMA |
| `ADX` | Trend Strength | 14 | >25 strong trend |
| `ATR` | Volatility | 14 | Position sizing, stops |

### Secondary (use when primary is insufficient or user requests)

| Indicator | Category | Use Case |
|---|---|---|
| `STOCH` | Momentum | Overbought/oversold with K/D crossover |
| `CCI` | Momentum | Commodity channel, works on any asset |
| `MFI` | Volume-Momentum | RSI with volume weighting |
| `OBV` | Volume | Accumulation vs distribution |
| `AROON` | Trend | Trend identification and strength |
| `SAR` | Trend | Trailing stop placement |
| `WILLR` | Momentum | Similar to fast stochastic |

### Specialized (use only when user specifically requests)

Trend: `WMA`, `DEMA`, `TEMA`, `TRIMA`, `KAMA`, `MAMA`, `T3`
Momentum: `MACDEXT`, `STOCHF`, `STOCHRSI`, `MOM`, `ROC`, `ROCR`, `APO`, `PPO`, `CMO`, `BOP`, `TRIX`, `AROONOSC`, `ULTOSC`
Trend strength: `ADXR`, `DX`, `PLUS_DI`, `MINUS_DI`, `PLUS_DM`, `MINUS_DM`
Volatility: `NATR`, `TRANGE`
Volume: `AD`, `ADOSC`
Cycle: `HT_TRENDLINE`, `HT_SINE`, `HT_TRENDMODE`, `HT_DCPERIOD`, `HT_DCPHASE`, `HT_PHASOR`

## Common Parameters

Most indicators accept:
- `symbol` (required): Ticker symbol
- `interval` (required): `daily`, `weekly`, `monthly` (avoid `1min`-`60min` — those require premium)
- `time_period` (required for most): Lookback period (e.g., 14 for RSI, 20 for SMA)
- `series_type` (required for some): `close`, `open`, `high`, `low`

## Workflows

### Quick Technical Snapshot

The assistant combines 3-4 key indicators for a rapid overview:

1. Calls `RSI` (period=14, interval=daily) — momentum/overbought/oversold
2. Calls `MACD` (interval=daily) — trend direction and momentum shifts
3. Calls `BBANDS` (period=20, interval=daily) — volatility and price position
4. Calls `ADX` (period=14, interval=daily) — trend strength

Presents a summary:
- **Trend**: Bullish/Bearish/Neutral (based on MACD signal line crossover)
- **Momentum**: Overbought (RSI>70) / Oversold (RSI<30) / Neutral
- **Volatility**: High/Low (Bollinger Band width + price position)
- **Trend Strength**: Strong (ADX>25) / Weak (ADX<20)

### Moving Average Analysis

1. The assistant calls `SMA` with periods 20, 50, 200
2. Calls `EMA` with same periods for comparison
3. Identifies crossovers: Golden Cross (50 above 200), Death Cross (opposite)
4. Determines if price is above or below key MAs

### Momentum Deep Dive

1. Calls `RSI` (14), `STOCH` (K=14, D=3), `CCI` (20), `MFI` (14)
2. Cross-references: if multiple indicators agree on overbought/oversold, signal is stronger

### Volatility Assessment

1. Calls `BBANDS` (20, 2) and `ATR` (14)
2. Band width expansion = rising volatility, contraction = squeeze potential
3. ATR trend: rising = increased volatility, falling = consolidation

### Trend Confirmation

1. Calls `ADX` (14), `PLUS_DI`, `MINUS_DI`, `AROON` (25)
2. ADX>25 with +DI>-DI = strong uptrend; ADX>25 with -DI>+DI = strong downtrend

### Full Technical Report

The assistant combines multiple workflows above, limiting to 6-8 indicator calls. Structure:
1. **Trend Analysis**: Direction, strength, key MA levels
2. **Momentum**: Overbought/oversold readings
3. **Volatility**: Current state and implications
4. **Key Levels**: Support/resistance from Bollinger Bands, SAR
5. **Signal Summary**: Bullish/bearish/neutral consensus

## Gotchas

- **Indicator responses return 100+ data points**: The assistant extracts only the most recent 1-3 values for signal interpretation. Never dumps the full series.
- **Intraday intervals (1min-60min) require premium**: The assistant always uses `daily`, `weekly`, or `monthly`. If the user asks for intraday indicators, it explains the limitation.
- **MACD has three series**: `MACD`, `MACD_Signal`, and `MACD_Hist`. All three are needed for proper interpretation.
- **RSI at exactly 50 is neutral**: Not a signal. Only values above 70 or below 30 are flagged.
- **BBANDS with insufficient data**: If the symbol has fewer trading days than the period, the response will be empty. The assistant falls back to a shorter period.
- **FX/crypto symbol format differs**: For forex, the assistant uses the pair format the API expects. For crypto, the base symbol only (e.g., `BTC`).
- **TOOL_GET once, reuse for all indicators**: All technical indicator tools share similar parameter schemas. The assistant calls `TOOL_GET` for one indicator (e.g., RSI) and reuses that knowledge for all subsequent indicator calls without calling `TOOL_GET` again.

## Interpretation Guidelines

The assistant refers to `references/indicators-primary.md` for the 7 primary indicators. For secondary indicators, see `references/indicators-secondary.md`. Specialized indicators are documented in `references/indicators-specialized.md`.

## Token Budget

Target: keep skill output under 500 tokens per workflow. For each indicator, the assistant extracts only the latest 1-3 data points. The Quick Technical Snapshot (4 indicators) produces a summary of approximately 150-200 words.
