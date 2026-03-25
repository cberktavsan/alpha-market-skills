# FX & Crypto Workflow Specifications

## Forex Deep Dive (4-6 API calls)

When the user asks about a currency pair's outlook:

1. forex-analysis: `CURRENCY_EXCHANGE_RATE` for current rate (1 call)
2. forex-analysis: `FX_DAILY` for recent price action (1 call — extract latest 5-10 days)
3. Optional technical overlay: `RSI` on the FX pair (1 call)
4. Optional technical overlay: `MACD` on the FX pair (1 call)

Present: current rate (both directions), daily trend summary, technical signals if requested, timestamp.

## Crypto Deep Dive (4-6 API calls)

When the user asks for comprehensive crypto analysis:

1. crypto-analysis: `CURRENCY_EXCHANGE_RATE` for current price (1 call)
2. crypto-analysis: `DIGITAL_CURRENCY_DAILY` for recent trend (1 call — extract latest 5-10 days)
3. Optional technical overlay: `RSI` on the crypto (1 call)
4. Optional technical overlay: `MACD` on the crypto (1 call)
5. Optional technical overlay: `BBANDS` on the crypto (1 call)

Present: current price, period trend, technical signal summary if requested.

## Multi-Currency Comparison (3-5 API calls)

When the user asks to compare multiple currencies:

1. forex-analysis: `CURRENCY_EXCHANGE_RATE` for each pair (1 call each, max 4-5 pairs)

Present: cross-rate table with rate, bid, ask. Highlight strongest and weakest currencies.

## Multi-Crypto Comparison (3-5 API calls)

When the user compares multiple cryptocurrencies:

1. crypto-analysis: `CURRENCY_EXCHANGE_RATE` for each coin vs USD (1 call each, max 4-5 coins)

Present: comparison table with current prices. For historical comparison, use daily data.
