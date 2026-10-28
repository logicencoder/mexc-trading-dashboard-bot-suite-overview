# MEXC Trading Dashboard and Bot Suite

![MEXC Trading Dashboard and Bot Suite — chart, order book, and bot panels](assets/bot-suite.png)

Manual trading, order follow-up and bot configuration often require the same market context. This MEXC workstation brings those tasks together so the operator can inspect the market, choose an action and follow its progress from one desk.

It combines a live trading interface with five configurable automation modes, saved settings and operating diagnostics. Manual and automated work remain visible parts of the same session.

## Tech stack

| Layer | Technologies |
|---|---|
| Application | Python |
| Trading interface | HTML, CSS, JavaScript |
| Exchange integration | MEXC, Protocol Buffers |

## Keep the market and account context together

The order book, recent-trade tape, balances and open orders sit close to the trading controls. This lets the operator inspect depth and current activity before choosing an amount and direction.

After submission, order feedback and the open-order view support following the action. A trade is therefore something to review through its lifecycle rather than a button press detached from the rest of the desk.

## Place and manage manual orders

Manual entry supports working with the selected market while keeping existing orders visible. Replacement and follow-up controls help the operator handle an order that needs adjustment.

Quantity can be expressed in coins or USDT where supported. Review the amount and current order context before proceeding, especially when several activities share the same account.

## Choose an automation mode

The suite offers distinct approaches: MODE1 reacts to a chosen price band with immediate-or-cancel limit orders; MODE2 places grid-style limits within a range; MODE3 uses a sequential re-entry ladder and stop price.

MODE4 maintains a resting limit near the best bid or ask and can re-quote when displaced. MODE5 schedules a single action for a selected time. Each mode has its own parameters so the selected behaviour can be reviewed deliberately.

## Reuse a reviewed setup

Presets make a familiar strategy easier to revisit, while optional dry-run support helps inspect a setup before live use. The operator can retain the settings relevant to a repeated task instead of entering them again from memory.

Saved configurations remain starting points for review. The current symbol, amount and selected mode still belong in the decision to start a session.

Amount limits, execution ranges and supported randomisation choices help express the intended strategy more precisely. Review them with the selected mode rather than assuming that every bot uses the same operating conditions.

## Keep account activity within reach

Balances, open orders, order history and funds-history views give the operator different ways to follow the account. Deposits and other recorded activity can be examined without leaving the trading workstation.

Chart choices, order highlighting and panel controls support a more focused session. Saved layout preferences make it easier to return to that arrangement after an interruption.

## Follow more than one bot

Multi-bot profiles support several independently configured activities. Their controls and runtime information help keep those activities distinguishable within the same workstation.

This is useful when comparing or supervising different setups, provided the operator continues to review their combined account context and intended use.

## Investigate feed and order issues

Timing information, debug events and runtime logs support examining an order that appears slow or a feed that has become interrupted. They keep the investigation close to the market and account state involved.

The workstation reduces the need to reconstruct an incident across unrelated tools. It provides trading and supervision controls without guaranteeing execution outcomes.

---

Private code: [mexc-trading-dashboard-bot-suite](https://github.com/logicencoder/mexc-trading-dashboard-bot-suite)

See [REPOS.md](REPOS.md) for related repositories.

**[Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
