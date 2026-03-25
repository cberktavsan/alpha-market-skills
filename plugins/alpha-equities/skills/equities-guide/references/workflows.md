# Equities Workflow Specifications

## Full Stock Report (9-12 API calls)

When the user asks for comprehensive analysis of a stock (triggers: "full analysis", "analyze [TICKER]", "[TICKER] analysis"):

1. stock-analysis: `GLOBAL_QUOTE` for current price (1 call, ~200 tokens — use in full)
2. fundamental-analysis: `COMPANY_OVERVIEW` for valuation metrics (1 call — extract only valuation fields)
3. fundamental-analysis: `INCOME_STATEMENT` for revenue/earnings trend (1 call — last 3 annual periods only)
4. fundamental-analysis: `BALANCE_SHEET` for financial health (1 call — last 3 annual periods only)
5. technical-analysis: Quick Technical Snapshot workflow (4 calls — RSI, MACD, BBANDS, SMA 200)
6. news-sentiment: `NEWS_SENTIMENT` with ticker for market sentiment (1 call — top 5 articles only)

Present in this order:
1. Price snapshot and daily change
2. Company identity and sector
3. Valuation table (P/E, P/B, P/S, EV/EBITDA)
4. Financial trend (revenue growth, margin trend, debt)
5. Technical signals (RSI, MACD, Bollinger, 200 SMA position)
6. News sentiment summary
7. Overall assessment: bullish / bearish / neutral signals tally

## Stock + News Intelligence (4-5 API calls)

When the user asks for a stock's current situation with news context:

1. stock-analysis: `GLOBAL_QUOTE` (1 call)
2. news-sentiment: `NEWS_SENTIMENT` with ticker (1 call)
3. news-sentiment: `INSIDER_TRANSACTIONS` (1 call)
4. Optional: fundamental-analysis: `COMPANY_OVERVIEW` (1 call)

Present: price snapshot, sentiment summary, insider activity, company context.

## Quick Technical + Price (4-5 API calls)

When the user asks for a ticker with technical analysis:

1. stock-analysis: `GLOBAL_QUOTE` (1 call)
2. technical-analysis: `RSI` 14 daily (1 call)
3. technical-analysis: `MACD` daily (1 call)
4. technical-analysis: `BBANDS` 20 daily (1 call)

Present: price, then technical signal summary (trend, momentum, volatility in 3-4 sentences).
