# Risk Limits — HIGH RISK team (AGGRESSIVE mode)

Set by the human on 2026-10-09. Switched 2026-10-14 to stocks only. **Switched
on 2026-11-18 to AGGRESSIVE mode: options and margin are in play again, with
full limits at all times (no drawdown ladder).** Read by the
`portfolio-manager` (the team's boss) and by the discipline guard in
`CLAUDE.md` on every request. Changes to this file are made by the human only.
**Every number is a maximum, not a target.**

## Goal

High return from a **concentrated-but-diversified book of long and short
stock and option positions**, using margin and defined-risk options for extra
punch, so that no single sector, position, squeeze, gap or earnings report can
do serious damage. Winners are pressed; losers are cut. Recovering a drawdown
is done with clean trades inside the limits, **never by sizing past them**.

## Losses do not lock you out (no ladder, no breakers)

The drawdown ladder was **removed by the human on 2026-11-18** and the loss
breakers (day -5% / week -12% no-new-trades) were **removed on 2026-11-23**.
Limits below apply in full at all times, and a losing day or week does not stop
new trades. Daily and weekly P&L and drawdown from the 60-day peak are still
reported in the boss summary, **for information only**. What still limits risk:
position size, open risk, delta-adjusted exposure, a stop on every trade, and
3 opening trades a day.

## Account-level limits 

All exposure is **delta-adjusted**: an option counts at delta × 100 × shares of
the underlying, a 2× ETF counts at 2×. An option or leveraged ETF cannot be used
to dodge a limit.

| Limit | Max |
|---|---|
| Account size used for sizing | Live account **equity** (net liquidation value), never buying power |
| Gross exposure (long + absolute short, delta-adjusted) | **2.0×** equity |
| Net long exposure (delta-adjusted) | **1.5×** equity |
| Net short exposure (delta-adjusted) | **0.75×** equity |
| Margin debit (borrowed cash) | **50%** of equity |
| Margin cushion | equity stays **≥ 30% above** the maintenance requirement; a margin call is met by cutting positions, never by depositing more |
| Total open risk (sum of entry → stop risk on stock, plus **100% of premium** on options) | **20%** |
| Total option premium open | **25%** of equity |
| Positions open at once | 12 (options on the same underlying as a stock count as one position) |

## Per-position limits 

| Limit | Max |
|---|---|
| Risk per stock trade (entry → stop, plus a normal overnight gap) | **4%** of equity |
| Option position premium | **5%** of equity per position (100% of it counts as at risk) |
| Long stock position size | **35%** of equity |
| Short stock position size | **25%** of equity (a short's loss has no ceiling) |
| Leveraged / inverse ETF | counts at its leverage toward every size limit; max 25% of equity at market value |
| One underlying, all instruments combined (stock + options + 2× ETF, delta-adjusted) | **40%** of equity |

## Deployment: idle cash is a cost (added 2026-11-24)

The human wants capital working. The boss sizes the book to **use the risk
budget** whenever vetted setups exist.

| Rule | Value |
|---|---|
| Deployment target | With 2+ signed-off setups, aim for **at least 60% of equity deployed** (delta-adjusted gross) and **8%+ open risk**, up to the limits above |
| Idle cash | If more than **50%** sits idle for 2+ sessions, the BOSS SUMMARY says why and names what would deploy it (a zone to be reached, a vetoed name that could be resized) |
| Not a licence | The target never overrides a veto, the 7.0 technical pass bar, the news check or the earnings exit dates. No entry is taken just to be invested. |

## Entry style: two tranches (updated 2026-11-24)

No breakouts or breakdowns (human rule). The old "wait for a rejection bar
before any order" is replaced by two tranches so price that touches the zone
does not run away without you:

| Tranche | Size | Order |
|---|---|---|
| A (starter) | 50% of the planned size | A **resting DAY limit inside the zone** (the zone's middle) from 9:45 ET on. No rejection bar needed. |
| B (confirmation) | 50% | On the rejection bar (a 30-minute bar into the zone that closes back beyond the zone's edge), or at the zone's far edge |

The stop is the same for the whole position and goes in as a GTC order the
moment any part fills. If price never reaches the zone: no trade, no chasing.
Within 0.5 ATR of the zone in a strong trend, the limit may sit at the zone's
near edge.

## Pressing winners (the aggressive part)

| Rule | Value |
|---|---|
| Add to a winner | Only when the position is **≥ +1R**, the stop has been moved to **breakeven or better**, and the new total risk stays inside the limits. Adds are sized so the combined position still risks ≤ 4% from the new stop. |
| Never add to a loser | Stock, option or ETF. Averaging down is a tripwire. |
| Exits | Take half at 2R, move the stop to breakeven, trail the rest (10- or 20-day average); no cap on the runner; 30-day time stop |
| Reward:risk to first target | **1.5:1** minimum for longs and shorts, **2:1** for options (premium risk to the underlying's T1 payoff) |

## Options (defined risk only)

| Rule | Value |
|---|---|
| Allowed structures | **Long calls, long puts, and debit vertical spreads.** Nothing with open-ended loss: **no naked or uncovered short options, no short straddles or strangles, no credit spreads** |
| Thesis | The option is only an instrument. The **underlying** must pass the same full team sign-off, entry zone and stop as a stock trade. No option on a name that failed any agent. |
| Expiration | Buy **30-75 DTE**, never closer than 2× the planned hold. **Exit by 14 DTE** at the latest. |
| Strike | **Delta 0.40-0.70.** Options below 0.25 delta (lottery tickets) are not allowed. Prefer a debit spread when IV is above its 60th percentile. |
| Liquidity | Bid-ask spread **≤ 10% of the mid** (or ≤ $0.10 under $1.00), open interest **≥ 100**. Orders are limits at or near the mid, never market orders. |
| Events | **Never through earnings, FDA or a regulatory decision** (IV collapse plus a gap). Out by the last close before. New entry needs 3+ trading days before that deadline. |
| Exit rules | Out if the underlying hits its stop, or the premium is **down 50%**, or 14 DTE, or day 30, whichever comes first. Take half at +100% premium or at the underlying's T1. |
| Resting stops | If the broker accepts a resting stop on the contract, enter it at fill. If not, set a price alert at the underlying stop and at -50% premium and exit by hand the same session. Say which one applies in the plan. |
| Count | At most **3 option positions** open at once; options + stock on one name = one position |
| No rolls | Never roll a losing option; a loser is closed |
| Tape | RED tape: no new long calls; puts and bear spreads on the weakest groups only. YELLOW: half the premium cap. |

## Short selling rules

| Rule | Value |
|---|---|
| Protective order | A **GTC buy-stop (buy to cover)** at the plan's invalidation is entered as soon as the short fills. No mental stops on shorts, ever. |
| Stop distance | 0.75-3 ATR above entry, above the structure that defines the breakdown |
| Shortable | Must be shortable and easy to borrow (`get_equity_tradability`); no hard-to-borrow names |
| Squeeze risk | Short interest **≤ 20%** of float and days to cover **≤ 5**; above either = no new short |
| Liquidity | ≥ **$20M** average daily $ volume and price ≥ **$10** |
| Event risk | No shorts with pending M&A / buyout talk or an activist; never hold a short through earnings (same exit-by rule) |
| Dividends | A short pays the dividend: flag any ex-dividend date inside the hold |
| Adding | Never add to a short that is above its average entry |

## Diversification and correlation

| Limit | Max |
|---|---|
| Gross exposure in one **sector bucket** (delta-adjusted) | **40%** of equity |
| Open risk in one sector bucket | **6%** of equity |
| Positions in one sector bucket | **3** |
| Correlated cluster (pairwise correlation > +0.60, signed P&L, same direction) | **40%** of equity delta-adjusted and **50%** of portfolio risk |
| Effective independent bets (PCA) when 4+ positions are open | **≥ 2.5** |
| 1-day 95% VaR (delta-adjusted) | **6%** of equity |
| Worst beta scenario (SPY -3%, QQQ -5%, SMH -8%, sector -10%) | **-8%** of equity |
| When 4+ long positions are open | hold at least one short (single name or index ETF) of ≥ 10% of equity, or an equal-delta put position |

**Sector buckets** (a name goes in the bucket it trades with, not its label):

1. AI / semis / hardware / data-center power (MU, MRVL, AXTI, NVDA, BE ...)
2. Software / internet / cybersecurity (MSFT, CRWD, ZS, OKTA ...)
3. Healthcare services and managed care (UNH, HUM, ELV, CVS ...)
4. Healthcare other: biotech, tools, diagnostics, medtech
5. Financials / fintech
6. Consumer / retail / staples
7. Industrials / energy / materials / defense
8. Crypto-linked (miners, exchanges, BTC ETFs)
9. Index hedges (SPY / QQQ / IWM shorts, puts or inverse ETFs)

## Earnings: up to, never through

| Rule | Value |
|---|---|
| Holding any position (long, short, stock or option) **through** an earnings report | **Never** |
| Holding **up to** earnings | **Allowed** - the run-up is fair game |
| Exit-by deadline | The close of the **last session before the report** (an after-the-close report: that day's close; a before-the-open report: the prior day's close) |
| New entry | Only if at least **3 trading days** remain before the exit-by deadline |
| Earnings date not confirmed | Treat the estimated date as real; if the timing is unknown, assume before the open |

## Instruments and exits

| Rule | Value |
|---|---|
| Instruments | Stocks and ETFs (incl. leveraged and inverse ETFs), long or short, cash or margin; long options and debit spreads per the Options section |
| Stock exits (long and short) | Structure stop entered as a GTC order at fill; take half at 2R; trail the rest beyond the 10- or 20-day average; 30-day time stop |
| Stock liquidity | ≥ $5M average daily $ volume for longs; ≥ $20M for shorts |
| Pattern Day Trader | Margin account under $25k: max **3 day trades** per 5 business days. A swing trade closed the same day it opened counts. |
| Margin cost | State the margin interest (and any borrow fee) for a trade planned to last more than 2 weeks |
| Falling knives | Never buy a stock or call on a name that fails the technical rubric (score < 7) or its sector gate, however "oversold" it looks. Oversold is not a signal. |
