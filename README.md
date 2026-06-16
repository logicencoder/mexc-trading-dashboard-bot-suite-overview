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

## Feature examples (two per capability)

#### Live MEXC Spot dashboard
1. You open the app, switch symbol to `DNXUSDT`, and see last price, live trades, and balances update without refresh.
2. The OrderBook badge turns green as protobuf depth messages arrive and the spread line populates.

#### Symbol search and favorites
1. You type `BTC` in the top bar, hit Search, and the app normalizes to `BTCUSDT`.
2. You star the symbol and a quick-switch button appears in the notification bar favorites row.

#### Account balances (real-time)
1. After a manual sell fills, Available drops and Locked may briefly rise until the private WS account update arrives.
2. You click **Hide** on balances before screen-sharing, then **Refresh** for a REST snapshot.

#### Live public trades feed
1. You watch the Live Trades panel tick with time, price, amount, and buy/sell coloring for the active symbol.
2. Trade count and last-price arrow update on each new deal from the public trades WebSocket.

#### Order book depth
1. You enable **My orders** highlight and see markers on book rows matching your open limit prices.
2. You click the ask **Price** header to re-sort depth rows before placing a manual limit.

#### TradingView chart
1. You keep default **TV** view and get an embedded chart for the current symbol.
2. You toggle **Custom** to see last-price summary while TradingView stays one click away.

#### Manual limit orders
1. You select **Buy**, **Limit**, enter price and qty, click **Buy**, and the order appears in Open Orders via WS + REST reconciliation.
2. You use the **75%** balance shortcut to size a sell without calculating coin qty manually.

#### Manual market orders
1. You select **Market**, set slippage to 0.5%, and submit a buy sized by the **$100** USDT preset.
2. A market sell uses coin quantity with validation hints if below min notional.

#### Cancel and modify open orders
1. From Activity → Open Orders, you click **C** and the order disappears after cancel confirms.
2. You click **M**, edit price/qty in the Trading Panel, and **Replacing…** runs cancel+replace with timing logged server-side.

#### Activity and order history
1. You toggle **Current** vs **All** to see only the active symbol’s opens or every symbol’s opens.
2. The **History** tab shows filled/canceled orders; **Refresh** pulls REST history for the view.

#### Bot logs in Activity
1. While MODE2 runs, you open **Bot Logs** and see “Placed LIMIT SELL…” lines with timestamps.
2. After stop, you still read recent bot log lines from persisted storage via the same tab.

#### MODE1 instant taking
1. You set SELL MODE1 range 0.045–0.050, style **TAKE**; the bot fires LIMIT IOC when a bid inside range appears.
2. Same range with **PING**: the bot posts GTC, waits ping hold ms, cancels, and logs finalize timing in Performance metrics.

#### MODE2 limit replenishment
1. You run MODE2 BUY with min 1.00, max 1.05; the bot rests a buy at 1.00 until filled, then places another.
2. With **Random Price** on, each new MODE2 order picks a random price inside the range instead of the min/max anchor.

#### MODE3 step ladder
1. You set start 0.100, step 2%, move lower; after each full fill the next sell limit steps down 2% from last fill.
2. You enable stop at 0.085 without “continue until budget”; the bot halts when the next price would cross the stop.

#### MODE4 front-of-book quoting
1. You run MODE4 SELL with jump 1 tick; the bot keeps a limit one tick inside best ask and re-quotes when displaced.
2. You adjust poll interval to 1.0s for faster reaction when the book is fast.

#### MODE5 scheduled exact order
1. You set Mode5 SELL at `18:30:00`, exact price 0.042, qty auto; countdown reaches zero and one limit is placed.
2. With **jump one tick** enabled, submit price nudges one tick using top-of-book before place if another order sits at target.

#### Bot safety, budget, and dry run
1. You cap **Total Amount** at 500 USDT; the bot sets safety halt when spent reaches cap.
2. You enable **Dry Run** and start MODE1; logs show `DRY RUN: would place…` with no REST order IDs.

#### Bot presets and multi-bot profiles
1. You save preset “Mode5 DNX 18:30” and reload it later before a scheduled launch.
2. On **Multi Bots**, you save profile “DNX ladder” (MODE3 + symbol) and start it while MODE4 runs on another symbol.

#### Funds notifications and history
1. A confirmed USDT deposit appears as a notification chip and in the expandable Funds panel.
2. On **Funds History**, you filter **Deposits** + asset `USDT` and open the explorer link when network matches.

#### Performance panel
1. You enable Live poll and watch `place_order` REST ms and `order_ws_latency_ms` after each bot fire.
2. You reset the view after a volatile session to compare fresh averages.

#### Debug Logs and System Stats
1. You filter Debug Logs to **order** + **ERROR** and search an orderId during a stuck-order incident.
2. System Stats shows stale feed count after a WS drop — you know which stream to reconnect.

#### UI layout persistence
1. You raise chart height to 600px in Settings, **Save Layout**, and panels stay arranged after reload.
2. **Snapshot UI** exports layout JSON for backup before a risky settings experiment.

---

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
