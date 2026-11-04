# TradingView Entry Tools

## Risk-managed pullback strategy

`pullback-strategy.pine` is a Pine Script v6 strategy for backtesting and paper practice. The original `early-entry.pine` indicator remains available.

Configured for **XAUUSD on 5-minute and 15-minute standard charts**. The higher timeframe filter is enabled by default: leave its timeframe input blank to use 15m confirmation on a 5m chart, or 1h confirmation on a 15m chart. You can override it with a strictly higher timeframe. Add the strategy separately to each chart; each instance manages its own trades and risk limits. Simultaneous signals on both charts do not share an account risk budget.

The default quantity step is 0.01 **TradingView symbol units/contracts**, not 0.01 broker lots. For gold, check your feed's point value and your broker's ounces per lot; one standard lot often represents 100 ounces, but specifications vary. Verify that a $1 gold move produces the expected P/L in Strategy Tester before sizing any broker order. No XAUUSD-specific profitability or parameter optimization is claimed.

### Entry model

- Long: price and the 20 EMA are above a rising 200 EMA; a prior candle in the last five bars touched or moved below the 20 EMA; the signal candle closes bullish above the previous high and the 20 EMA.
- Short: the corresponding conditions below a falling 200 EMA, followed by a bearish close below the previous low.
- ADX must be at least 20 and directional movement must agree with the trade.
- The higher timeframe filter uses only the previous completed higher timeframe candle. Select a timeframe strictly above the chart timeframe if overriding the automatic setting.
- Optional session filter uses the symbol exchange timezone. It limits entry submissions, not exits or position holding; a next-bar fill may occur outside the session.
- One position at a time; no pyramiding or automatic reversal. Defaults allow three entry submissions per exchange day with three full bars of cooldown after exit.

### Risk and orders

- Default planned risk: 0.5% of current equity per trade, before costs. Quantity is rounded down to the chosen quantity step and capped at 100% notional exposure. A zero quantity skips the trade.
- Set **Quantity step** to the instrument's actual order increment (for example, 1 share/contract or 0.001 units). Quantity means TradingView symbol units/contracts, not universally broker lots. Verify point value, currency, minimum quantity, and margin against the broker before use. The unleveraged defaults may skip futures or expensive instruments.
- Stop distance uses the recent five-bar swing plus a 0.2 ATR buffer, with a minimum of 1 ATR. Distances above 3 ATR are rejected. The profit target defaults to 2R. Distances are rounded to valid ticks.
- Stop and target orders use tick distances from the actual simulated entry fill. They can therefore differ from the original swing prices after a gap.
- TradingView's intraday equity-loss rule defaults to 2% from the intraday equity peak, including unrealized P/L. It cancels pending orders, closes positions, and blocks further orders for the remainder of the trading session. It can act before the per-trade stop. On chart timeframes above daily, TradingView treats a bar as a day for this rule; use intraday charts for intraday practice.
- BUY/SELL labels mark entry **submissions at candle close**. Actual fills appear in TradingView's trade markers and trade list, usually at the next bar open. The risk rule or margin checks may prevent a submitted order from filling.
- Entry sizing is estimated at signal close. Gaps, slippage, costs, and stop execution can increase realized risk; next-open price changes may also cause margin rejection.
- Stop/target lines appear on subsequent strategy calculations while a position remains open. A trade that opens and closes within one bar may have no visible level lines.

### Install and practice

1. Copy `pullback-strategy.pine` into TradingView Pine Editor, save, and select Add to chart. Use standard candlestick charts.
2. In Strategy Properties, set your account capital/currency and actual commission, slippage, and margin. Defaults are 10,000 initial capital, 0.05% commission on each fill, two ticks of slippage, and 100% margin. Commission does not separately model a variable spread; account for spread in your assumptions.
3. In Inputs, set quantity step, direction, session, and timeframe filters for your instrument. Start with the 0.5% risk setting in paper practice.
4. Review Strategy Tester trade fills, drawdown, net profit after costs, average trade, and results across trending and sideways periods. Keep a separate, untouched out-of-sample period. Avoid choosing settings from the best historical profit alone.
5. Test with Bar Magnifier where available to improve historical stop/target sequencing, then forward-test in paper trading. Without intrabar data, a candle touching both stop and target is resolved by TradingView's broker-emulator assumptions.
6. For notifications, create a strategy alert on **order fills** and include `{{strategy.order.alert_message}}` in the alert message. Alerts do not by themselves execute broker trades; historical fills do not send live alerts.

### Validation status

Source and risk-sizing arithmetic were reviewed locally. This environment has no TradingView Pine compiler or market-data backtest, so compilation, performance, and broker execution have not been verified. This is a practice framework, not a proven profitable strategy. Test in TradingView and paper trading before considering live use.

## Original entry indicator

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
