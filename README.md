# Numinous Tools

A skill pack wrapping the [Numinous](https://numinouslabs.io) forecasting APIs. Works in Claude Code (as a plugin), clawhub, Cursor, Cline, or any agent that reads a `SKILL.md`.

Gives your AI agent one command to:
- Generate calibrated AI probability forecasts on any binary event (pay per call, or with a pre-funded credit balance)
- Pull LLM-scored signals for a Polymarket market or free-text question
- Track balance and usage, browse miners, inspect agent code

## Install

### Claude Code (plugin — recommended)

```
/plugin marketplace add infinite-mech/numinous-tools
/plugin install numinous@numinous
```

Then `/reload-plugins` if needed. Commands become `/numinous:forecast`, etc. — but you rarely need to call them directly; just ask Claude for a forecast and the skill will activate.

### Drop-in (Claude Code, Cursor, Cline, any agent that reads `SKILL.md`)

```
git clone https://github.com/infinite-mech/numinous-tools ~/.claude/skills/numinous-src
ln -s ~/.claude/skills/numinous-src/skills/numinous ~/.claude/skills/numinous
```

### Via clawhub

```
npx clawhub@latest install infinite-mech/numinous-tools
```

## Configure

After install, set either (or both):

```bash
# API key (recommended — works for forecasts, signals, balance, usage)
export NUMINOUS_API_KEY=<create at https://eversight.numinouslabs.io/api-keys>

# Optional: x402 crypto payments (forecasts only, no signals)
export NUMINOUS_X402_EVM_PRIVATE_KEY=<hex; wallet must hold USDC on Base>
```

Add to `~/.zshrc` / `~/.bashrc` or a project `.env`. Never commit.

## Use

Just ask your agent — the skill auto-activates on Numinous-related requests:

> "Give me a Numinous forecast on whether BTC will exceed $150k by end of 2026."
>
> "What's my Numinous balance?"
>
> "Get signals on this Polymarket: https://polymarket.com/event/..."
>
> "Who's the top miner right now?"

The agent handles auth, routing, polling, and presenting results. See [`skills/numinous/SKILL.md`](skills/numinous/SKILL.md) for the full capability list and [`skills/numinous/reference.md`](skills/numinous/reference.md) for API details.

## What's in the repo

```
.claude-plugin/
  plugin.json          Claude Code plugin manifest
  marketplace.json     Single-plugin marketplace
skills/numinous/
  SKILL.md             Main skill — agent reads this for routing
  reference.md         Deep API reference (lazy-loaded)
  scripts/
    forecast.py        Submit + poll a prediction (API key)
    forecast_x402.py   Submit + poll a prediction (x402 USDC)
    signals.py         Get signals for a market or question
```

## Costs

Fetched live from the public costs endpoint — the skill never hardcodes.

```bash
curl -s https://api-eversight.numinouslabs.io/api/v1/credits/costs
```

At time of writing: **$0.10** per forecast, **$0.025** per signal request, **$1** minimum top-up.

## Links

- **Landing:** https://numinouslabs.io
- **Leaderboard UI:** https://leaderboard.numinouslabs.io
- **Eversight (balance, top-up, API keys):** https://eversight.numinouslabs.io
- **API docs (Swagger):** https://api.numinouslabs.io/api/docs

## License

MIT-0
