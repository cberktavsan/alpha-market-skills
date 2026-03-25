---
name: economic-dashboard
description: Retrieves and analyzes US macroeconomic indicators including GDP, CPI, unemployment, Fed rate, and treasury yields.
metadata:
  version: "1.0.0"
---

# Economic Dashboard

The assistant accesses and analyzes US macroeconomic indicators from Alpha Vantage.

> **Global rules (language, rate limits, free-tier constraints, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Indicator | Frequency |
|------|-----------|-----------|
| `REAL_GDP` | Real Gross Domestic Product (annualized) | Quarterly |
| `REAL_GDP_PER_CAPITA` | Real GDP per capita | Quarterly |
| `TREASURY_YIELD` | US Treasury bond yields (2Y, 5Y, 7Y, 10Y, 30Y) | Daily/Weekly/Monthly |
| `FEDERAL_FUNDS_RATE` | Fed Funds effective rate | Daily/Weekly/Monthly |
| `CPI` | Consumer Price Index | Monthly/Semiannual |
| `INFLATION` | Annual inflation rate | Annual |
| `RETAIL_SALES` | Retail and food services sales | Monthly |
| `DURABLES` | Durable goods orders (manufacturers) | Monthly |
| `UNEMPLOYMENT` | Civilian unemployment rate | Monthly |
| `NONFARM_PAYROLL` | Total nonfarm employment | Monthly |

## Common Parameters

- `interval`: `daily`, `weekly`, `monthly`, `quarterly`, `semiannual`, `annual` (varies by indicator)
- `maturity`: For treasury yields — `3month`, `2year`, `5year`, `7year`, `10year`, `30year`

## Workflows

### Economic Snapshot

The assistant builds a quick overview of the US economy:

1. Calls `FEDERAL_FUNDS_RATE` (interval=monthly) — current Fed rate
2. Calls `CPI` (interval=monthly) — latest inflation reading
3. Calls `UNEMPLOYMENT` (interval=monthly) — current unemployment
4. Calls `REAL_GDP` (interval=quarterly) — latest GDP growth

Presents a concise dashboard with brief interpretation: expanding/contracting, inflation hot/cooling.

### Interest Rate Analysis

1. Calls `FEDERAL_FUNDS_RATE` (monthly) for current and historical Fed rate
2. Calls `TREASURY_YIELD` for multiple maturities (2Y, 10Y, 30Y)
3. Calculates yield curve spread: 10Y - 2Y
   - Positive spread: Normal curve (economic expansion expected)
   - Negative spread (inverted): Historically predicts recession
4. Shows rate trajectory over recent months

### Inflation Deep Dive

1. Calls `CPI` (interval=monthly) for recent readings
2. Calls `INFLATION` for annual rate
3. Calculates month-over-month and year-over-year changes
4. Context: Fed target is 2% annual inflation

### Labor Market Analysis

1. Calls `UNEMPLOYMENT` (monthly) for current and trend
2. Calls `NONFARM_PAYROLL` (monthly) for jobs added/lost
3. Interprets together: falling unemployment + strong NFP = tight labor market
4. Historical context: <4% unemployment is generally considered full employment

### GDP Trend

1. Calls `REAL_GDP` (quarterly) for recent quarters
2. Calls `REAL_GDP_PER_CAPITA` (quarterly) for per-capita view
3. Calculates QoQ growth rates
4. Two consecutive negative quarters = technical recession

### Consumer Spending Check

1. Calls `RETAIL_SALES` (monthly)
2. Calls `DURABLES` (monthly) for big-ticket spending
3. Interprets: rising retail + rising durables = confident consumer

## Gotchas

- **REAL_GDP is quarterly with ~2 month lag**: The "latest" GDP reading is typically 1-2 months old. The assistant always states which quarter it covers (e.g., "Q3 2025 GDP, released November 2025").
- **CPI monthly vs INFLATION annual**: CPI gives monthly index values (the assistant calculates YoY change manually). INFLATION gives the pre-calculated annual rate. Use INFLATION for quick checks, CPI for trend analysis.
- **TREASURY_YIELD requires `maturity` parameter**: Omitting it may return unexpected results. The assistant always specifies: `3month`, `2year`, `5year`, `7year`, `10year`, or `30year`.
- **Yield curve inversion detection**: The assistant calls TREASURY_YIELD twice (2year and 10year) and compares. A 10Y-2Y spread below zero indicates inversion.
- **NONFARM_PAYROLL is revision-heavy**: The initial NFP release is frequently revised. The assistant notes that the data point may be revised.
- **These are all US indicators**: If the user asks about non-US economic data, the assistant explains that Alpha Vantage economic endpoints cover only US data.
- **FEDERAL_FUNDS_RATE vs TREASURY_YIELD**: The Fed funds rate is the overnight interbank rate set by the Fed. Treasury yields are market-determined. The assistant does not conflate them.
- **RETAIL_SALES and DURABLES are nominal**: Not inflation-adjusted. Rising retail sales during high inflation may not indicate real growth.

## Output Format

- The assistant leads with the most market-relevant number (usually Fed rate or CPI)
- Shows trends over time, not just the latest reading
- Includes the date of each data point
- Provides context: historical average, Fed target, previous reading
- Notes that these are US-specific indicators

## Token Budget

Target: keep skill output under 500 tokens. For economic snapshots, a 4-5 line dashboard (one line per indicator) plus a 2-3 sentence interpretation.
