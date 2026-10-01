# claude_code_agents_high_risk

The **high-risk** version of [`claude_code_agents`](https://github.com/jstudz69/claude_code_agents):
the same 7-agent team for swing trades held 1 to 30 days, retuned to hunt
aggressive, asymmetric setups.

> ⚠️ This team takes bigger, more volatile, option-heavy bets. Losses can be
> fast and large. Every trade still has a defined max loss — and you still
> place every order yourself.

## What's different from the standard team

| Area | Standard | High risk |
|---|---|---|
| Instruments | Stocks/ETFs | + long calls/puts, debit spreads, leveraged ETFs |
| Direction | Long only (in practice) | Long **and** bearish (via puts) |
| Liquidity floor | $20M ADV | $5M ADV; options OI ≥ 500, spread ≤ 10% |
| Earnings / catalysts | Exit before | Allowed as deliberate, defined-risk event trades |
| Chase limit | Trigger + 0.5 ATR | Trigger + 1 ATR |
| Extension allowed | < 3 ATR over 20DMA | < 4 ATR if money flow confirms |
| Technical screen pass bar | 8.0 / 10 | 7.0 / 10 (`policy/tech-screen-rubric.md`) |
| Candidates per run | ~3-5 | Up to 10 long + 5 bearish |
| Risk limits (risk manager, on request) | 0.5%/trade, 3% total, no options | 2%/trade, 15% total, options ≤ 5% premium each / 30% total |
| Option exits | n/a | -50% premium stop, half off at +100%, out 7 days before expiry |

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
