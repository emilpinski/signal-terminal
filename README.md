# Signal Terminal — Solana Signal Bot

> Real-time alpha signals for Solana pump.fun tokens.

**Status:** Live — 362 signals tracked, avg peak multiplier 5.4x

---

## What it does

- Real-time monitoring of new token creations on pump.fun via WebSocket
- KOL (Key Opinion Leader) wallet tracking — alerts when smart money enters a position
- Volume spike and momentum pattern detection pre and post graduation
- Structured Telegram alerts with entry price, market cap, liquidity, buy/sell ratio

## How it works

- **PumpPortal WebSocket**: Connects to `wss://pumpportal.fun/api/data` to stream new token creations and graduation events in real time
- **Multi-stage risk analysis**: Fetches RugCheck safety reports, Helius RPC data for holder concentration, and DexScreener market data — scores tokens 0–100 with aggressive filtering
- **Spike scanner**: Detects volume/price anomalies pre-graduation; queues tokens for 15-minute settlement before final scoring to avoid false signals from initial buy volatility
- **Smart money tracker**: Monitors configurable KOL wallet list via PumpPortal; copies signals when tracked wallets enter positions

## Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![asyncio](https://img.shields.io/badge/asyncio-grey) ![Telegram Bot API](https://img.shields.io/badge/Telegram_Bot_API-26A5E4?logo=telegram&logoColor=white) ![Solana](https://img.shields.io/badge/Solana-9945FF?logo=solana&logoColor=white)
