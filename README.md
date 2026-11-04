# TradingView Early Entry

Pine Script v6 indicator displaying BUY and SELL text labels on the chart.

## Installation

Copy early-entry.pine into TradingView's Pine Editor, save, and select Add to chart. For alerts, select BUY Entry or SELL Entry and Once Per Bar Close.

## Rules

- BUY: bullish candle closes above the previous candle's high, optionally above the trend EMA.
- SELL: bearish candle closes below the previous candle's low, optionally below the trend EMA.
- Only the first candle in each consecutive breakout sequence qualifies. A filtered signal does not trigger later in the same sequence.
- Signals confirm at candle close. The EMA filters direction without requiring a crossover.
- EMA length defaults to 50. The filter can be disabled in settings.
- Up to 500 labels are retained; TradingView removes older labels automatically.

## Validation

Not compiled or tested in TradingView. This is an indicator, not a backtesting strategy. Signals do not guarantee profitable trades or execution prices.
