# claude_code_agents_high_risk

The **high-risk** version of [`claude_code_agents`](https://github.com/jstudz69/claude_code_agents):
the same 7-agent team for swing trades held 1 to 30 days, retuned to hunt
aggressive, asymmetric setups.

> ⚠️ This team takes bigger, more volatile bets: long **and short** stock on
> **margin**. Losses can be fast and large, and a short's loss has no ceiling.
> Every trade still has a stop resting at the broker — and you still place
> every order yourself.

## What's different from the standard team

| Area | Standard | High risk |
|---|---|---|
| Instruments | Stocks/ETFs | Stocks/ETFs incl. leveraged and inverse ETFs, on margin. No options (since 2026-10-14) |
| Direction | Long only (in practice) | Long **and** short stock |
| Liquidity floor | $20M ADV | $5M ADV longs; $20M ADV and ≥ $10 for shorts |
| Earnings / catalysts | Exit before | Held up to the report, never through |
| Chase limit | Trigger + 0.5 ATR | Trigger + 1 ATR |
| Extension allowed | < 3 ATR over 20DMA | < 4 ATR if money flow confirms |
| Technical screen pass bar | 8.0 / 10 | 7.0 / 10 (`policy/tech-screen-rubric.md`) |
| Candidates per run | ~3-5 | Up to 10 long + 5 bearish |
| Risk limits (risk manager, on request) | 0.5%/trade, 3% total, no options | 2.5%/trade, 15% total open risk, gross ≤ 1.5× equity, shorts ≤ 50% of equity |
| Shorts | n/a | GTC buy-stop at fill, easy to borrow, short interest ≤ 20%, days to cover ≤ 5 |

| Agent | File | Job |
|---|---|---|
| Head orchestrator | `.claude/agents/trading-orchestrator.md` | Runs the process, synthesizes, presents the plan |
| Macro analyst | `.claude/agents/macro-analyst.md` | Market regime |
| Sector analyst | `.claude/agents/sector-analyst.md` | Sector rotation and candidates |
| Fundamentals analyst | `.claude/agents/fundamentals-analyst.md` | Single-stock business and valuation |
| Technical analyst | `.claude/agents/technical-analyst.md` | Trend, levels, momentum, ATR |
| Entry manager | `.claude/agents/entry-manager.md` | Order previews (never places orders) |
| Risk manager | `.claude/agents/risk-manager.md` | Sizing and exposure review - **on request only**, advisory |

Shared rules, the pipeline and the report format are in [`CLAUDE.md`](CLAUDE.md).

## Setup

1. Open this repo in Claude Code.

## Usage

Run the orchestrator as the main agent so it can delegate to the others:

```bash
claude --agent trading-orchestrator
```

Then ask, for example:

- `Full analysis on NVDA, long`
- `What sectors look best right now? Find me 2 long candidates`
- `Just the macro regime today` (the orchestrator will call only the macro analyst)

You can also call any specialist directly from a normal session:
`Use the technical-analyst agent on AAPL`.

The risk manager only runs when you ask, e.g. `Run a risk review on my
positions` or `Size an IQV entry at 272.76 with a stop at 264.70`. Its limits
live in `policy/risk-limits.md` (aggressive defaults; edit to taste).

## Day trader (intraday, on request)

`.claude/agents/day-trader.md` is a standalone intraday technician. Ask it
directly, e.g. `Use the day-trader agent: pre-market plan for MXL` or
`day-trader: live check on HPE`. It builds a level map (prior-day and
pre-market highs/lows, VWAP, pivots, opening range, relative volume), names
the setup (ORB, VWAP reclaim, EMA pullback, gap-and-go, failed breakout),
and gives entry / stop / 1R-2R targets and share size. Everything is flat by
the close. It checks the **Pattern Day Trader rule** first: margin accounts
under $25k get 3 day trades per 5 business days.

## Discipline guard ("yell at me when I start gambling")

Before any analysis that touches the account or a trade idea, the agent you
are talking to pulls your recent orders (read-only) and checks the tripwires in
[`policy/discipline-rules.md`](policy/discipline-rules.md): entries before
9:45, averaging down, oversized positions, margin/leverage breaches, shorts
without a buy-stop, trading through a loss breaker, churn, revenge
re-entries, off-plan trades, chasing and new option trades. Any hit is
shown first as a blunt **STOP** alarm with the order and dollars. It never
blocks analysis; on a RED alarm new ideas are marked "watchlist only".

## Safety design

- No agent has order-placing tools. The entry manager produces previews; you
  place orders yourself.
- Each agent's `tools:` line is an allowlist. Agents have **read-only
  Robinhood tools** only (quotes, historicals, indicators, fundamentals,
  earnings, positions, balance, order *review*). No agent has
  `place_*`, `cancel_*` or any other order tool. Never add them.
- The tool names use the prefix `mcp__Robhinhood__`, which is the name of
  the Robinhood connector in the Claude account this was built with. If your
  connector has a different name (check `/mcp` in Claude Code), find-and-replace
  the prefix in `.claude/agents/*.md`.
- The repo is public: agents keep account details out of saved reports.

## Not financial advice

These agents produce research for your own decisions.
