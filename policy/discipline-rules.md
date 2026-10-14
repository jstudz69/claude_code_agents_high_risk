# Discipline Rules (the "gambling tripwires")

The human asked to be **called out loudly** when they start gambling. Every
agent checks these tripwires against the live account before doing anything
else on a request that touches the account or a trade idea. See "Discipline
guard" in `CLAUDE.md` for how to respond.

These rules were written after a week in which these exact behaviors caused
most of the losses. Changes to this file are made by the human only.

## Tripwires

| # | Tripwire | How to detect (Robinhood, read-only) | Why it's here |
|---|---|---|---|
| 1 | **Early entry**: an opening order (buy or sell short) before 9:45 ET | `get_equity_orders`, opening orders with `created_at` 9:30-9:45 ET | Opening-range noise. Plans say 9:45. |
| 2 | **Averaging down**: adding to a losing position - buying more of a red long, or shorting more of a short trading above its average entry | Opening order on a symbol where the existing long's mark < average cost, or the existing short's mark > average entry | Turns small losses into big ones. Losers are held, trimmed or exited only. |
| 3 | **Oversized position**: a stock risking > 2.5% of equity entry → stop (> 1% during probation), or a long > 20% / short > 15% of equity (short > 10% during probation) | Order quantity × price and stop vs. `get_portfolio` equity | Per-position caps in `risk-limits.md`. |
| 4 | **Leverage blow-out**: gross exposure > 1.5× equity, net long > 1.25× or net short > 0.5×, margin debit > 50% of equity, margin cushion < 30% over maintenance, or any margin call | `get_portfolio` (equity, market value, margin / buying power) + `get_equity_positions` (longs and shorts) | Margin turns a bad week into a wiped-out account. |
| 5 | **Naked short**: a short stock position with no resting GTC buy-stop, a short in a name with short interest > 20% of float or days to cover > 5, or a short that is hard to borrow | `get_equity_positions` (negative qty) vs. open buy-stop orders in `get_equity_orders`; `get_equity_tradability`; fundamentals' short interest | A short's loss has no ceiling; a squeeze gaps straight through a mental stop. |
| 6 | **Trading through a breaker**: any opening order after the day is down 4%+ or the rolling week 10%+ | `get_portfolio`, `get_realized_pnl` / `get_pnl_trade_history` | Revenge trading. After a breaker only reducing trades are allowed. |
| 7 | **Churn**: more than 3 opening trades in one day, opening and closing the same symbol within 30 minutes, or a 4th day trade in 5 business days while under $25k (PDT) | Today's and the last 5 business days' orders | Impulse trading, paying the spread for nothing, and a 90-day PDT lock. |
| 8 | **Revenge re-entry**: re-opening a name within 2 trading days of exiting it at a loss | Recent orders + realized P/L | Emotional, not planned. |
| 9 | **Off-plan trade**: an opening order on a name that is not in that day's written plan (if one exists in `reports/<date>/`) | Compare orders to the plan | The plan is written calm; the trade is made in the moment. |
| 10 | **Chasing**: entry more than 1 ATR past the planned trigger | Fill price vs. plan trigger and ATR | High-risk chase limit. |
| 11 | **Earnings exposure**: holding anything past its exit-by deadline (the last close before its report), or opening a position with fewer than 3 trading days before that deadline | `get_earnings_results` per held / ordered symbol | The human's rule: up to earnings, never through. |
| 12 | **Sector pile-up**: more than 2 positions, gross exposure > 35% of equity or open risk > 4% in one sector bucket | Positions mapped to the buckets in `risk-limits.md` | One sector must not be able to wreck the account. |
| 13 | **Option trade**: any new opening option order (options are off since 2026-10-14), or adding to / rolling a leftover option | `get_option_orders`, opening orders | The human switched the team to stocks only. |

## Severity

- **RED** (any of 2, 4, 5, 6, 11, or three or more tripwires at once): lead with the
  alarm, list every hit with the order and the dollars, and give only
  reduce/exit actions. New entry ideas may still be shown, but labelled
  **WATCHLIST ONLY - NOT TODAY**.
- **AMBER** (one or two of the others): lead with the alarm, then proceed.
- **CLEAR**: one line, "Discipline check: clear", then proceed.

## Standing limits (in addition to `risk-limits.md`)

| Limit | Value |
|---|---|
| Max new opening trades per day | 3 |
| Probation, until 10 clean trading days are logged | stock risk ≤ 1% per trade; short size ≤ 10% of equity |
| No new trades | after a breaker trips, for the rest of that day |
| Entries | only after 9:45 ET, only from the written plan, only at or inside the planned entry zone |
| Shorts | the GTC buy-stop goes in right after the fill, before anything else |
