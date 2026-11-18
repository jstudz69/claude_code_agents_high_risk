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
| 3 | **Oversized position**: a stock risking more than the ladder tier allows (3% of equity at FULL, scaled by tier), an option position over its premium cap (5% of equity at FULL), a long > 25% / short > 15% of equity, or one underlying > 30% of equity delta-adjusted (stock + options + 2× ETF) | Order quantity × price and stop vs. `get_portfolio` equity; `get_option_positions` for premium and delta | Per-position caps in `risk-limits.md`. |
| 4 | **Leverage blow-out**: **delta-adjusted** gross exposure > 2.0× equity, net long > 1.5× or net short > 0.75×, margin debit > 50% of equity, margin cushion < 30% over maintenance, or any margin call. Options count at delta, 2× ETFs at 2× | `get_portfolio` + `get_equity_positions` + `get_option_positions` / `get_option_quotes` (delta) | Margin and options turn a bad week into a wiped-out account. |
| 5 | **Naked short**: a short stock position with no resting GTC buy-stop, a short in a name with short interest > 20% of float or days to cover > 5, or a short that is hard to borrow | `get_equity_positions` (negative qty) vs. open buy-stop orders in `get_equity_orders`; `get_equity_tradability`; fundamentals' short interest | A short's loss has no ceiling; a squeeze gaps straight through a mental stop. |
| 6 | **Trading through a breaker or a tier**: any opening order after the day is down 5%+ or the rolling week 12%+, or any opening order the current drawdown tier forbids (REDUCE-ONLY: none; QUARTER: no options, no new margin; HALF: no non-leaders) | `get_portfolio`, `get_realized_pnl` / `get_pnl_trade_history`, the drawdown ladder in `risk-limits.md` | Revenge trading. After a breaker only reducing trades are allowed. |
| 7 | **Churn**: more than 3 opening trades in one day, opening and closing the same symbol within 30 minutes, or a 4th day trade in 5 business days while under $25k (PDT) | Today's and the last 5 business days' orders | Impulse trading, paying the spread for nothing, and a 90-day PDT lock. |
| 8 | **Revenge re-entry**: re-opening a name within 2 trading days of exiting it at a loss | Recent orders + realized P/L | Emotional, not planned. |
| 9 | **Off-plan trade**: an opening order on a name that is not in that day's written plan (if one exists in `reports/<date>/`) | Compare orders to the plan | The plan is written calm; the trade is made in the moment. |
| 10 | **Chasing**: entry more than 1 ATR past the planned trigger | Fill price vs. plan trigger and ATR | High-risk chase limit. |
| 11 | **Earnings exposure**: holding anything past its exit-by deadline (the last close before its report), or opening a position with fewer than 3 trading days before that deadline | `get_earnings_results` per held / ordered symbol | The human's rule: up to earnings, never through. |
| 12 | **Sector pile-up**: more than 3 positions, delta-adjusted gross > 40% of equity or open risk > 6% in one sector bucket, or a correlated cluster (pairwise > +0.60, same direction) > 40% of equity | Positions mapped to the buckets in `risk-limits.md`; correlation per `portfolio-manager` | One sector must not be able to wreck the account. |
| 13 | **Option outside the rules**: an option that is not a long call, long put or debit spread; under 30 DTE or over 75; delta under 0.40 or over 0.70; spread > 10% of mid or open interest < 100; held through earnings or a binary event; a 4th option position; a market order; **adding to or rolling a losing option**; or an option opened on a name with no team sign-off | `get_option_orders`, `get_option_positions`, `get_option_quotes`, `get_earnings_results` | Options are back in play only as defined-risk instruments of a signed-off trade. |
| 14 | **Falling-knife buy**: an opening long or call on a name that fails the technical rubric (< 7), its sector gate or the fundamentals news check (BLOCK) - "it's oversold" is not a signal | Compare the order to that day's team reports | Buying a drop with no setup, because it "feels oversold", is how a fast drawdown becomes a deep one. |
| 15 | **Sizing up to recover**: any opening order larger than the tier allows, or larger than the previous trade, within 5 trading days of a loss of 5%+ | Order size vs. the previous trade and the ladder | The goal is recovery; the method is clean trades at ladder size, never bigger bets. |

## Severity

- **RED** (any of 2, 4, 5, 6, 11, 13, 14, 15, or three or more tripwires at once): lead with the
  alarm, list every hit with the order and the dollars, and give only
  reduce/exit actions. New entry ideas may still be shown, but labelled
  **WATCHLIST ONLY - NOT TODAY**.
- **AMBER** (one or two of the others): lead with the alarm, then proceed.
- **CLEAR**: one line, "Discipline check: clear", then proceed.

## Standing limits (in addition to `risk-limits.md`)

| Limit | Value |
|---|---|
| Max new opening trades per day | 3 at FULL, 2 at HALF, 1 at QUARTER, 0 at REDUCE-ONLY |
| Drawdown ladder | FULL / HALF / QUARTER / REDUCE-ONLY by drawdown from the 60-day equity peak; see `risk-limits.md`. Replaces the old probation rule. |
| No new trades | after a breaker trips, until the next clean trading day; the account then restarts at QUARTER |
| Entries | only after 9:45 ET, only from the written plan, only at or inside the planned entry zone |
| Shorts | the GTC buy-stop goes in right after the fill, before anything else |
