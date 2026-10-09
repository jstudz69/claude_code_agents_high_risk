# Risk Limits — HIGH RISK team

Used only by the `risk-manager`, which runs **only when you ask for it** and
gives advice, not a veto. These are the aggressive defaults for this repo —
edit any value to taste. No other agent reads this file.

| Limit | Value |
|---|---|
| Account size used for sizing | Live account value from Robinhood |
| Max risk per trade (stock: entry → stop; option: premium paid) | **2%** of account |
| Max position size, stock (% of account) | **15%** |
| Max premium per option position (% of account) | **5%** |
| Max total option premium (% of account) | **30%** |
| Max total open risk (sum of all stop / premium risk) | **15%** |
| Max exposure to one sector or theme | **40%** |
| Max correlated positions (same theme) | **3** |
| Daily loss before new trades stop | **4%** |
| Weekly loss before new trades stop (rolling 7 days) | **10%** |
| Instruments | Stocks, ETFs (incl. leveraged), long calls and long puts only (no spreads) |
| Naked short options / short stock / margin | **No** |
| Minimum reward:risk to first target | **1.5 : 1** |
| Minimum avg daily $ volume (stock) | **$5M** |
| Option liquidity | Open interest ≥ 500, spread ≤ 10% of mid |
| Holding through earnings / events | **Allowed** if the risk is defined (option premium, or a stock position sized so a 2x-normal gap stays within the per-trade limit) |
| Option exits | -50% premium stop; half off at +100%; close 7 days before expiry |
| Time stop | 30 days (stocks) |
