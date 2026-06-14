# MEXC Trading Dashboard & Bot Suite

Production-grade **MEXC spot** dashboard — manual trading plus **five automation modes** (`MODE1`–`MODE5`), multi-bot profiles, and execution diagnostics in one self-hosted stack.

Private source: [logicencoder/mexc-trading-dashboard-bot-suite](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite). API credentials via local `.env` — never committed.

## The problem it solves

Discretionary trades and scripted bots usually live in separate tools — exchange UI for manual work, ad-hoc scripts for automation. This suite combines **realtime book + account panels**, **order placement/modify/cancel**, and **mode-specific bot engines** with shared WebSocket ingestion and anti-stale order filtering after cancel/modify.

## Manual trading UX

Realtime orderbook, trades tape, balances, and open orders. Place and replace orders with immediate lifecycle feedback. The UI reconciles private account protobuf frames with REST snapshots so state drift is visible before it becomes a costly mistake.

## Bot engine — MODE1 through MODE5

| Mode | Summary |
|------|---------|
| **MODE1** | Book-reactive limit IOC when price enters your band |
| **MODE2** | Grid-style limit orders within a range |
| **MODE3** | Sequential re-entry ladder with stop price |
| **MODE4** | Resting limit at best bid/ask with re-quote when displaced |
| **MODE5** | Scheduled single-shot order at configured HH:MM:SS |

Each mode has isolated parameters, runtime logs, preset save/load, and optional dry-run. **Multi-bot profiles** run parallel independent bots without shared global state. Mode5 supports scheduled triggers; quantity can be expressed in coins or USDT.

## Diagnostics

Timing metrics and debug-event persistence for modify-order latency and feed issues — evidence when MEXC API or WebSocket quirks make orders look stuck.

## Stack

| Layer | Role |
|-------|------|
| `mexc_trading_app.html` + `mexc_trading_app.js` | UI state, WS rendering, bot controls |
| `mexc_trading_app.py` | REST, MEXC signing, persistence |
| `bot_engine.py` | MODE1–MODE5 execution |
| `generated_proto/` | Exchange stream decode |

Default dev URL: `http://127.0.0.1:8005`.

## Related product

[mexc_trading_app-overview](https://github.com/logicencoder/mexc_trading_app-overview) covers the leaner **MODE1 + MODE2** build when you do not need the full mode suite.

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
