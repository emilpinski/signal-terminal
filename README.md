# Signal Terminal

> Real-time Solana pump.fun signal bot — KOL wallet tracking, volume spike detection, Telegram alerts.

![Screenshot](https://raw.githubusercontent.com/emilpinski/signal-terminal/main/docs/screenshots/signalbot_telegram.jpg)

## What is it

Signal Terminal is an async Python bot that monitors new tokens created on pump.fun in real time via WebSocket PumpPortal. Every new token is automatically scored based on: rug check, holder concentration, KOL wallet activity, and volume. Tokens with a score above the threshold are delivered as signals to a Telegram channel with a full briefing (mcap, score, links to DexScreener and Birdeye).

Built for traders and crypto enthusiasts looking for early signals on pump.fun before the pump.

## Features

- **Real-time WebSocket** — PumpPortal `wss://pumpportal.fun/api/data`, zero latency on new tokens
- **Scoring engine** — multi-factor scoring: rug check (RugCheck.xyz), holder concentration (Helius RPC), market cap, initial buy
- **KOL Wallet Tracker** — tracking known smart money wallets, bonus score when a KOL buys
- **Smart Money Tracker** — analyzing token creator wallet profit history
- **Volume Spike Detector** — detecting sudden volume surges after graduation on Raydium/PumpSwap
- **Migration monitoring** — tracking tokens reaching $69K mcap and migrating to Raydium
- **Telegram signals** — formatted alerts with inline buttons (DexScreener, Birdeye, pump.fun)
- **SQLite persistence** — token history, signals, scan statistics (aiosqlite)
- **Dashboard** — local web UI with bot statistics and signal history
- **Auto-reconnect** — WebSocket with exponential backoff on disconnect
- **Win rate tracking** — ~26% win rate from 362+ scanned signals

## Stack

| Layer | Technology |
|-------|-----------|
| Runtime | Python 3.11+ |
| Telegram | python-telegram-bot v21+ (async Application API) |
| WebSocket | websockets + aiohttp |
| Database | SQLite (aiosqlite) |
| HTTP | aiohttp (async) |
| Data | PumpPortal WS, DexScreener API, RugCheck API, Helius RPC |
| Deploy | Linux daemon (systemd / screen) |

## Getting Started

```bash
git clone https://github.com/emilpinski/signal-terminal
cd signal-terminal
pip install -r requirements.txt
cp .env.example .env
# Fill in environment variables
python run_v2.py
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `TELEGRAM_BOT_TOKEN` | Telegram bot token | ✅ |
| `TELEGRAM_CHANNEL_ID` | Telegram channel/group ID | ✅ |
| `HELIUS_RPC_URL` | Helius RPC URL (with API key) | ✅ |
| `MIN_SCORE` | Minimum score for signal (default 60) | ❌ |
| `HOT_SIGNAL_SCORE` | Score for "HOT" signal (default 80) | ❌ |

## Architecture

```
signal-terminal/
├── src/
│   ├── bot.py          # Telegram bot, signal delivery, commands
│   ├── scanner.py      # Orchestrator: WebSocket → enrich → score → signal
│   ├── scorer.py       # Multi-factor scoring engine
│   ├── kol_tracker.py  # KOL wallet tracking
│   ├── smart_money.py  # Token creator wallet history analysis
│   ├── volume_spike.py # Post-graduation spike detection
│   ├── rugcheck.py     # RugCheck.xyz client
│   ├── dexscreener.py  # DexScreener API client
│   ├── helius.py       # Helius RPC client
│   └── db.py           # SQLite persistence
├── run_v2.py           # Entry point
└── dashboard/          # Local web UI
```

## Status

Live — @SignalTerminalBot | ~26% win rate, 362+ signals

---
Built by [Emil Piński](https://emilpinski.pl)
