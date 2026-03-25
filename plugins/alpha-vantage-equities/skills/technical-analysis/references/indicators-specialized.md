# Specialized Indicator Notes

Specialized indicators are advanced variants of primary and secondary indicators. The assistant uses `TOOL_GET` to retrieve their parameter schemas on demand.

## Trend Variants

`WMA`, `DEMA`, `TEMA`, `TRIMA`, `KAMA`, `MAMA`, `T3` — These behave like SMA/EMA but with different smoothing algorithms. DEMA/TEMA reduce lag. KAMA adapts to volatility. T3 is the smoothest. Interpretation follows the same MA principles (price above = bullish, crossovers, support/resistance).

## Momentum Variants

`MACDEXT` — MACD with customizable MA types. `STOCHF`, `STOCHRSI` — faster/more sensitive versions of Stochastic/RSI. `MOM`, `ROC`, `ROCR` — raw momentum and rate of change. `APO`, `PPO` — absolute/percentage price oscillators. `CMO`, `BOP`, `TRIX`, `AROONOSC`, `ULTOSC` — various momentum measurements. Same overbought/oversold principles apply.

## Directional Movement

`ADXR`, `DX`, `PLUS_DI`, `MINUS_DI`, `PLUS_DM`, `MINUS_DM` — Components of the ADX system. +DI > -DI = bullish, -DI > +DI = bearish. ADX/ADXR measure strength only.

## Hilbert Transform (Cycle Analysis)

`HT_TRENDLINE`, `HT_SINE`, `HT_TRENDMODE`, `HT_DCPERIOD`, `HT_DCPHASE`, `HT_PHASOR` — Advanced cycle detection. Use only when the user specifically requests cycle analysis. These are rarely needed for standard technical analysis.
