# Macro Workflow Specifications

## Economic Snapshot (4-5 API calls)

When the user asks "how is the economy?" or "economic outlook" (brief version):

1. economic-dashboard: `FEDERAL_FUNDS_RATE` (monthly) for current Fed rate (1 call)
2. economic-dashboard: `CPI` (monthly) for latest inflation reading (1 call)
3. economic-dashboard: `UNEMPLOYMENT` (monthly) for current unemployment (1 call)
4. economic-dashboard: `REAL_GDP` (quarterly) for latest GDP growth (1 call)

Present as a compact dashboard: Fed rate, CPI YoY, unemployment, GDP growth, with brief interpretation (expanding/contracting, inflation hot/cooling).

## Macro + Market Pulse (6-8 API calls)

When the user asks about the full economic outlook:

1. economic-dashboard: `REAL_GDP` for GDP growth (1 call)
2. economic-dashboard: `CPI` (monthly) for inflation (1 call)
3. economic-dashboard: `UNEMPLOYMENT` for labor market (1 call)
4. economic-dashboard: `FEDERAL_FUNDS_RATE` for monetary policy (1 call)
5. economic-dashboard: `TREASURY_YIELD` (2year) for short-term rates (1 call)
6. economic-dashboard: `TREASURY_YIELD` (10year) for long-term rates (1 call)
7. commodity-tracker: `WTI` for energy costs (1 call)

Present: GDP trend → inflation status → labor market → Fed stance → yield curve shape (and whether inverted: 10Y-2Y spread) → energy cost signal.

## General Market Overview (5-7 API calls)

When the user asks "how is the market?" or "market overview":

1. market-overview: `MARKET_STATUS` for which markets are open (1 call)
2. market-overview: `TOP_GAINERS_LOSERS` for today's movers (1 call)
3. market-overview: `GLOBAL_QUOTE` for SPY as S&P 500 proxy (1 call)
4. economic-dashboard: `FEDERAL_FUNDS_RATE` (monthly) for current rate (1 call)
5. economic-dashboard: `TREASURY_YIELD` (10year, daily) for bond market (1 call)

Present: market status → index level → top 5 movers per category → macro context.

## Commodity Dashboard (5-6 API calls)

When the user wants a full commodity overview:

1. commodity-tracker: `WTI` (daily) (1 call)
2. commodity-tracker: `BRENT` (daily) (1 call)
3. commodity-tracker: `NATURAL_GAS` (daily) (1 call)
4. commodity-tracker: `GOLD_SILVER_SPOT` (1 call)
5. commodity-tracker: `COPPER` (1 call)

Present: energy section (WTI, Brent, natural gas with WTI-Brent spread), metals section (gold, silver with gold/silver ratio, copper). Single summary table with commodity, price, unit, direction.
