# MEXC Trading Dashboard and Bot Suite

![MEXC Trading Dashboard and Bot Suite — chart, order book, and bot panels](assets/bot-suite.png)

Manual trading, order follow-up and bot configuration often require the same market context. This MEXC workstation brings those tasks together so the operator can inspect the market, choose an action and follow its progress from one desk.

**What:** FastAPI backend, rich browser dashboard, and `bot_engine` running **MODE1–MODE5** automation on MEXC spot — order placement, modify/cancel, open orders, balances, and websocket-driven book/trades.  
**Why:** Operators need one surface for discretionary trades and scripted bots without switching between exchange UI and ad-hoc scripts.  
**Who:** Logic Encoder operator with MEXC API keys; not a hosted SaaS for third parties.

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

© Logic Encoder. Exchange use subject to MEXC terms.
