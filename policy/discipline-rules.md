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
| 1 | **Early entry**: an opening order before 9:45 ET | `get_equity_orders` / `get_option_orders`, opening orders with `created_at` 9:30-9:45 ET | Opening-range noise. Plans say 9:45. |
| 2 | **Averaging down**: buying more of something that is red | Opening order on an underlying where the existing position's mark < average cost | Turns small losses into big ones. Losers are held, trimmed or exited only. |
| 3 | **Oversized position**: one new option > 5% of the account in premium (> $700 during probation), or a stock risking > 2% (> 1% during probation) | Order premium × quantity vs. `get_portfolio` total | Per-position caps in `risk-limits.md`. |
| 4 | **Leverage blow-out**: one position's delta-dollar exposure > 1.5× the account | `get_option_quotes` delta × 100 × qty × underlying price | A "hedge" bigger than the whole account is a bet, not a hedge. |
| 5 | **Options over cap**: total option premium > 40% of the account | `get_portfolio` `options_value` / `total_value` | Portfolio cap in `risk-limits.md`. |
| 6 | **Trading through a breaker**: any opening order after the day is down 4%+ or the rolling week 10%+ | `get_portfolio`, `get_realized_pnl` / `get_pnl_trade_history` | Revenge trading. After a breaker only reducing trades are allowed. |
| 7 | **Churn**: more than 3 opening trades in one day, or opening and closing the same contract within 30 minutes | Today's orders | Impulse trading, paying the spread for nothing. |
| 8 | **Revenge re-entry**: re-opening a name within 2 trading days of exiting it at a loss | Recent orders + realized P/L | Emotional, not planned. |
| 9 | **Off-plan trade**: an opening order on a name that is not in that day's written plan (if one exists in `reports/<date>/`) | Compare orders to the plan | The plan is written calm; the trade is made in the moment. |
| 10 | **Chasing**: entry more than 1 ATR past the planned trigger | Fill price vs. plan trigger and ATR | High-risk chase limit. |
| 11 | **Earnings exposure**: holding anything past its exit-by deadline (the last close before its report), or opening a position with fewer than 3 trading days before that deadline | `get_earnings_results` per held / ordered symbol | The human's rule: up to earnings, never through. |
| 12 | **Sector pile-up**: more than 2 positions or more than 10% of the account in one sector bucket | Positions mapped to the buckets in `risk-limits.md` | One sector must not be able to wreck the account. |

## Severity

- **RED** (any of 2, 4, 6, 11, or three or more tripwires at once): lead with the
  alarm, list every hit with the order and the dollars, and give only
  reduce/exit actions. New entry ideas may still be shown, but labelled
  **WATCHLIST ONLY - NOT TODAY**.
- **AMBER** (one or two of the others): lead with the alarm, then proceed.
- **CLEAR**: one line, "Discipline check: clear", then proceed.

## Standing limits (in addition to `risk-limits.md`)

| Limit | Value |
|---|---|
| Max new opening trades per day | 3 |
| Probation, until 10 clean trading days are logged | option premium ≤ $700; stock risk ≤ 1% |
| No new trades | after a breaker trips, for the rest of that day |
| Entries | only after 9:45 ET, only from the written plan, only at or inside the planned buy zone |
