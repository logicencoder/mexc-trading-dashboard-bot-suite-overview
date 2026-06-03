# MEXC Trading Dashboard & Bot Suite — overview

Public description of the **MEXC Trading Dashboard & Multi-Mode Bot** — manual spot trading UI plus automated strategy modes on MEXC. Private code: [mexc-trading-dashboard-bot-suite](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite).

## What it is

**What:** FastAPI backend, rich browser dashboard, and `bot_engine` running **MODE1–MODE5** automation on MEXC spot — order placement, modify/cancel, open orders, balances, and websocket-driven book/trades.  
**Why:** Operators need one surface for discretionary trades and scripted bots without switching between exchange UI and ad-hoc scripts.  
**Who:** LogicEncoder operator with MEXC API keys; not a hosted SaaS for third parties.

## Manual trading UX

**What:** Real-time orderbook, trades tape, balances, open orders; place and replace orders with immediate lifecycle feedback and anti-stale filtering after cancel/modify.  
**Why:** Latency and state drift cause costly mistakes; the UI reconciles websocket events with REST snapshots.  
**Who:** Operator executing discretionary trades during volatile sessions.

## Bot engine (MODE1–MODE5)

**What:** Mode-specific parameters, runtime logs, scheduled triggers (Mode5 time-based), side and quantity units (coins vs USDT), preset save/load, **multi-bot profiles** for parallel independent runs.  
**Why:** Different strategies (scalp, grid-like, scheduled dump/buy) should not share one global state machine.  
**Who:** Operator testing automation with clear mode boundaries before leaving bots unattended.

## Architecture (private repo)

| Layer | Files | Role |
|-------|-------|------|
| UI | `mexc_trading_app.html`, `mexc_trading_app.js` | State, WS rendering, bot controls |
| API | `mexc_trading_app.py` | REST, MEXC signing, persistence, diagnostics |
| Engine | `bot_engine.py` | MODE1–MODE5 execution |
| Protos | `generated_proto/` | Exchange stream decode |

Default dev URL: `http://127.0.0.1:8005`. Credentials via `.env` (`MEXC_API_KEY`, `MEXC_API_SECRET`) — never committed.

## Diagnostics

**What:** Timing metrics and debug-event persistence for modify-order latency and feed issues.  
**Why:** MEXC API and WS quirks require evidence when orders look “stuck”.  
**Who:** Operator tuning execution during incidents.

## Related repositories

| Repo | Role |
|------|------|
| [mexc-trading-dashboard-bot-suite](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite) | Private — this product tree |
| [mexc_trading_app](https://github.com/logicencoder/mexc_trading_app) | Related MEXC trading codebase |
| [mexc-live-stats-plugin](https://github.com/logicencoder/mexc-live-stats-plugin) | WordPress live stats (Hostinger) |

See [REPOS.md](REPOS.md).

## Licensing

© LogicEncoder. Exchange use subject to MEXC terms.
