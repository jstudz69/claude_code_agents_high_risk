# Risk Limits — HIGH RISK team

Set by the human on 2026-10-09; **switched on 2026-10-14 to stocks only, margin
enabled, short selling allowed (no new options).** Read by the `portfolio-manager`
(on request) and by the discipline guard in `CLAUDE.md` (every request).
Changes to this file are made by the human only. **Every number is a maximum,
not a target.**

## Goal

High return from a **diversified book of long and short stock positions**,
using margin for extra gross exposure, so that no single sector, position,
squeeze or earnings report can do serious damage. Positions may run **up to**
earnings but are always **out before the report**.

## Account-level limits

| Limit | Max |
|---|---|
| Account size used for sizing | Live account **equity** (net liquidation value) from Robinhood, never buying power |
| Gross exposure (long value + absolute short value) | **1.5×** equity |
| Net long exposure | **1.25×** equity |
| Net short exposure | **0.5×** equity |
| Total short value | **50%** of equity |
| Margin debit (borrowed cash) | **50%** of equity |
| Margin cushion | equity stays **≥ 30% above** the maintenance requirement; a margin call is met by cutting positions, never by depositing more |
| Total open risk (sum of entry → stop risk, longs and shorts) | **15%** |
| Positions open at once | 10 |
| Daily loss before new trades stop | **4%** |
| Weekly loss before new trades stop (rolling 7 days) | **10%** |

## Per-position limits

| Limit | Max |
|---|---|
| Risk per trade (entry → stop, plus a normal overnight gap) | **2.5%** of equity |
| Long position size | **20%** of equity |
| Short position size | **15%** of equity (a short's loss has no ceiling) |
| Probation (until 10 clean trading days are logged) | stock risk ≤ **1%** per trade; short size ≤ **10%** |

## Short selling rules

| Rule | Value |
|---|---|
| Protective order | A **GTC buy-stop (buy to cover)** at the plan's invalidation is entered as soon as the short fills. No mental stops on shorts, ever. |
| Stop distance | 1-3 ATR above entry, above the structure that defines the breakdown |
| Shortable | Must be shortable and easy to borrow (`get_equity_tradability`); no hard-to-borrow names |
| Squeeze risk | Short interest **≤ 20%** of float and days to cover **≤ 5**; above either = no new short |
| Liquidity | ≥ **$20M** average daily $ volume and price ≥ **$10** |
| Event risk | No shorts with pending M&A / buyout talk or an activist; never hold a short through earnings (same exit-by rule) |
| Dividends | A short pays the dividend: flag any ex-dividend date inside the hold |
| Adding | Never add to a short that is above its average entry (that is averaging down) |

## Diversification

| Limit | Max |
|---|---|
| Gross exposure in one **sector bucket** | **35%** of equity |
| Open risk in one sector bucket | **4%** of equity |
| Positions in one sector bucket | **2** (a long and a short in the same bucket count as two) |
| When 4+ long positions are open | hold at least one short (single name or index ETF) of ≥ 10% of equity |

**Sector buckets** (a name goes in the bucket it trades with, not its label):

1. AI / semis / hardware / data-center power (MU, MRVL, AXTI, NVDA, BE ...)
2. Software / internet / cybersecurity (MSFT, CRWD, ZS, OKTA ...)
3. Healthcare / biotech / medtech
4. Financials / fintech
5. Consumer / retail / staples
6. Industrials / energy / materials / defense
7. Crypto-linked (miners, exchanges, BTC ETFs)
8. Index hedges (SPY / QQQ / IWM shorts or inverse ETFs)

## Earnings: up to, never through

| Rule | Value |
|---|---|
| Holding any position (long or short) **through** an earnings report | **Never** |
| Holding **up to** earnings | **Allowed** - the run-up is fair game |
| Exit-by deadline | The close of the **last session before the report** (an after-the-close report: that day's close; a before-the-open report: the prior day's close) |
| New entry | Only if at least **3 trading days** remain before the exit-by deadline |
| Earnings date not confirmed | Treat the estimated date as real; if the timing is unknown, assume before the open |

## Instruments and exits

| Rule | Value |
|---|---|
| Instruments | Stocks and ETFs (incl. leveraged and inverse ETFs), **long or short**, cash or margin |
| Options | **No new option positions.** Options opened before 2026-10-14 are only reduced or closed under their old exits (-50% premium stop, sell at +100%, out 7 days before expiry, out before earnings); never added to or rolled |
| Stock exits (long and short) | Structure stop entered as a GTC order at fill; take half at 2R; trail the rest beyond the 10- or 20-day average; 30-day time stop |
| Minimum reward:risk to first target | **1.5 : 1** for longs, **2 : 1** for shorts |
| Stock liquidity | ≥ $5M average daily $ volume for longs; ≥ $20M for shorts |
| Pattern Day Trader | Margin account under $25k: max **3 day trades** per 5 business days. A swing trade closed the same day it opened counts. |
| Margin cost | State the margin interest (and any borrow fee) for a trade planned to last more than 2 weeks |
