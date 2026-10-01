# Technical screen scorecard — HIGH RISK (10 points)

Use completed daily bars only (Robinhood `get_equity_historicals`, day, ~1 year).
Compute everything in Python. Benchmark: SPY. Score longs as written; for
bearish (put) candidates, mirror every rule (below falling MAs, outflow,
underperforming, breakdown setup).

| # | Factor | Points | Rule (long) |
|---|---|---|---|
| 1 | Trend | 0-3 | +1 close > rising 50DMA; +1 50DMA > 200DMA (or close > 200DMA if < 200 bars); +1 10DMA > 20DMA > 50DMA |
| 2 | Money flow | 0-2 | Count: 20d net $ flow % > +10; 20d up/down vol > 1.3; CMF(20) > +0.05; OBV 20d slope > 0. 4/4 = 2, 3/4 = 1.5, 2/4 = 1, else 0 |
| 3 | Relative strength vs SPY | 0-2 | +1 if 1M return beats SPY by > 3 pts; +1 if 3M beats by > 5 pts (0.5 each for a smaller beat) |
| 4 | Setup / extension | 0-2 | 2 = clean setup within 1.5 ATR of its trigger AND < 4 ATR above 20DMA; 1 = extended 4-5 ATR or trigger 1.5-2.5 ATR away; 0 = > 5 ATR extended, broken, or no setup |
| 5 | Momentum | 0-1 | RSI(14) 50-80 AND MACD > signal (0.5 if only one) |
| + | Squeeze bonus | +0.5 (cap 10) | Short interest > 15% of float with price above a key level on rising volume |

**Trigger** = the pattern's breakout level (flag/base/range high), not a single
day's high, measured from the latest official close.

**Hard gates (score capped at 5):** 20-day avg daily $ volume < $5M; price < $1;
fewer than 60 daily bars.

**Pass = 7.0 or higher.**
