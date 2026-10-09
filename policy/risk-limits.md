# Risk Limits — HIGH RISK team

Set by the human on 2026-10-09. Read by the `risk-manager` (on request) and by
the discipline guard in `CLAUDE.md` (every request). Changes to this file are
made by the human only. **Every number is a maximum, not a target.**

## Goal

High return from a **diversified mix of long options and high-momentum
stocks**, so that no single sector, position or earnings report can do serious
damage. Positions may run **up to** earnings but are always **out before the report**.

## Account-level limits

| Limit | Max |
|---|---|
| Account size used for sizing | Live account value from Robinhood |
| Total option premium deployed | **40%** of the account (cash is the rest) |
| Total open risk (sum of stop / premium-at-stop risk) | **15%** |
| Positions open at once | 10 |
| Daily loss before new trades stop | **4%** |
| Weekly loss before new trades stop (rolling 7 days) | **10%** |

## Per-position limits

| Limit | Max |
|---|---|
| Risk per trade (stock: entry → stop; option: premium × 50%, i.e. to the -50% stop) | **2.5%** of the account |
| Option premium per position | **5%** of the account (about **$700** at ~$14k) |
| Stock position size | **15%** of the account |
| Stock-equivalent exposure of one option (delta × 100 × qty × price) | **1.5×** the account |
| Probation (until 10 clean trading days are logged) | option premium ≤ **$700**; stock risk ≤ **1%** |

## Diversification

| Limit | Max |
|---|---|
| Premium + stock value in one **sector bucket** | **10%** of the account |
| Positions in one sector bucket | **2** |
| Premium expiring in the same week | **50%** of total premium |
| When 4+ bullish positions are open | hold at least one put or index hedge (≤ 5% premium) |

**Sector buckets** (a name goes in the bucket it trades with, not its label):

1. AI / semis / hardware / data-center power (MU, MRVL, AXTI, NVDA, BE ...)
2. Software / internet / cybersecurity (MSFT, CRWD, ZS, OKTA ...)
3. Healthcare / biotech / medtech
4. Financials / fintech
5. Consumer / retail / staples
6. Industrials / energy / materials / defense
7. Crypto-linked (miners, exchanges, BTC ETFs)
8. Index hedges (SPY / QQQ / IWM puts)

## Earnings: up to, never through

| Rule | Value |
|---|---|
| Holding any position (stock or option) **through** an earnings report | **Never** |
| Holding **up to** earnings | **Allowed** - the run-up is fair game |
| Exit-by deadline | The close of the **last session before the report** (an after-the-close report: that day's close; a before-the-open report: the prior day's close) |
| New entry | Only if at least **3 trading days** remain before the exit-by deadline |
| Earnings date not confirmed | Treat the estimated date as real; if the timing is unknown, assume before the open |

## Instruments and exits

| Rule | Value |
|---|---|
| Instruments | Stocks, ETFs (incl. leveraged), **long calls and long puts only** - no spreads |
| Naked short options / short stock / margin | **No** |
| Option expiry at entry | **≥ 30 days** |
| Option exits, 2+ contracts | -50% premium stop; sell half at +100%; close 7 days before expiry |
| Option exits, 1 contract | -50% stop; at **+50%** set a profit stop at **+20%**; sell at **+100%**; close 7 days before expiry |
| Stock exits | Structure stop set at entry; sell half at 2R; trail the rest under the 10- or 20-day average; 30-day time stop |
| Minimum reward:risk to first target | **1.5 : 1** |
| Stock liquidity | ≥ $5M average daily $ volume |
| Option liquidity | Open interest ≥ 500, bid/ask ≤ 10% of mid |
