# Primary Indicator Interpretation Guide

## RSI (Relative Strength Index)

- **Period**: Typically 14
- **Range**: 0-100
- **Overbought**: >70 (potential sell signal)
- **Oversold**: <30 (potential buy signal)
- **Divergence**: Price makes new high but RSI doesn't = bearish divergence (and vice versa)
- **Centerline**: 50 acts as support in uptrends, resistance in downtrends

## MACD (Moving Average Convergence Divergence)

- **Default**: Fast=12, Slow=26, Signal=9
- **Signal Line Crossover**: MACD crosses above signal = bullish, below = bearish
- **Zero Line Crossover**: MACD crosses above 0 = bullish momentum, below = bearish
- **Histogram**: Shows distance between MACD and signal line; shrinking histogram = momentum fading
- **Divergence**: Price trend vs MACD trend disagreement = potential reversal

## Bollinger Bands (BBANDS)

- **Default**: Period=20, StdDev=2
- **Upper Band Touch**: Overbought / strong uptrend continuation
- **Lower Band Touch**: Oversold / strong downtrend continuation
- **Band Squeeze**: Narrowing bands = low volatility, often precedes a big move
- **Band Width**: (Upper - Lower) / Middle — quantifies volatility
- **%B**: (Price - Lower) / (Upper - Lower) — position within bands (0=lower, 1=upper)

## SMA / EMA (Moving Averages)

- **SMA**: Simple average, equal weight to all periods
- **EMA**: Exponential, more weight to recent data (reacts faster)
- **Key Periods**: 20 (short-term), 50 (medium-term), 200 (long-term)
- **Golden Cross**: 50-period MA crosses above 200-period MA = strong bullish signal
- **Death Cross**: 50-period MA crosses below 200-period MA = strong bearish signal
- **Price vs MA**: Price above 200 SMA = long-term uptrend, below = downtrend
- **MA Stack**: 20>50>200 = bullish alignment, 20<50<200 = bearish alignment

## ADX (Average Directional Index)

- **Period**: Typically 14
- **Range**: 0-100 (measures trend strength only, not direction)
- **<20**: Weak trend or ranging market
- **20-25**: Emerging trend
- **25-50**: Strong trend
- **50-75**: Very strong trend
- **>75**: Extremely strong trend (rare, often unsustainable)
- **Direction**: Use +DI and -DI for direction; ADX only measures strength

## ATR (Average True Range)

- **Period**: Typically 14
- **Interpretation**: Higher ATR = more volatility; does NOT indicate direction
- **Use Cases**: Position sizing, stop-loss placement, volatility comparison
- **ATR Trailing Stop**: Common formula: Close - (2 * ATR) for long positions
