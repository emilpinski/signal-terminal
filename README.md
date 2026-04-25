# Signal Terminal

> Real-time Solana pump.fun signal bot — KOL wallet tracking, volume spike detection, Telegram alerts.

![Screenshot](./screenshot.png)

## Co to jest

Signal Terminal to asynchroniczny bot napisany w Pythonie, który monitoruje w czasie rzeczywistym nowe tokeny tworzone na pump.fun przez WebSocket PumpPortal. Każdy nowy token jest automatycznie scorowany na podstawie: rug check, koncentracji holderów, aktywności KOL walletów i wolumenu. Tokeny z wynikiem powyżej progu trafiają jako sygnały do kanału Telegram z pełnym briefingiem (mcap, score, linki do DexScreener i Birdeye).

Projekt zbudowany dla traderów i krypto-entuzjastów szukających early signals na pump.fun przed pump-em.

## Funkcje

- **Real-time WebSocket** — PumpPortal `wss://pumpportal.fun/api/data`, zero opóźnienia przy nowych tokenach
- **Scoring engine** — wieloczynnikowy scoring: rug check (RugCheck.xyz), holder concentration (Helius RPC), market cap, initial buy
- **KOL Wallet Tracker** — śledzenie znanych smart money walletów, bonus score gdy KOL kupuje
- **Smart Money Tracker** — analiza historii zysku walletów twórców tokenów
- **Volume Spike Detector** — wykrywanie nagłych wzrostów wolumenu po graduation na Raydium/PumpSwap
- **Migration monitoring** — śledzenie tokenów osiągających $69K mcap i przechodzących na Raydium
- **Telegram signals** — sformatowane alerty z inline buttons (DexScreener, Birdeye, pump.fun)
- **SQLite persistence** — historia tokenów, sygnałów, statystyki skanowania (aiosqlite)
- **Dashboard** — lokalne web UI ze statystykami botów, historią sygnałów
- **Auto-reconnect** — WebSocket z exponential backoff przy rozłączeniu
- **Win rate tracking** — ~26% win rate z 362+ zeskanowanych sygnałów

## Stack

| Warstwa | Technologia |
|---------|-------------|
| Runtime | Python 3.11+ |
| Telegram | python-telegram-bot v21+ (async Application API) |
| WebSocket | websockets + aiohttp |
| Baza danych | SQLite (aiosqlite) |
| HTTP | aiohttp (async) |
| Dane | PumpPortal WS, DexScreener API, RugCheck API, Helius RPC |
| Deploy | Linux daemon (systemd / screen) |

## Uruchomienie

```bash
git clone https://github.com/emilpinski/signal-terminal
cd signal-terminal
pip install -r requirements.txt
cp .env.example .env
# Uzupelnij zmienne srodowiskowe
python run_v2.py
```

## Zmienne środowiskowe

| Zmienna | Opis | Wymagana |
|---------|------|----------|
| `TELEGRAM_BOT_TOKEN` | Token bota Telegram | ✅ |
| `TELEGRAM_CHANNEL_ID` | ID kanalu/grupy Telegram | ✅ |
| `HELIUS_RPC_URL` | URL Helius RPC (z API key) | ✅ |
| `MIN_SCORE` | Minimalny score dla sygnalu (domyslnie 60) | ❌ |
| `HOT_SIGNAL_SCORE` | Score dla "HOT" sygnalu (domyslnie 80) | ❌ |

## Architektura

```
signal-terminal/
├── src/
│   ├── bot.py          # Telegram bot, delivery sygnałów, komendy
│   ├── scanner.py      # Orchestrator: WebSocket → enrich → score → signal
│   ├── scorer.py       # Wieloczynnikowy scoring engine
│   ├── kol_tracker.py  # Śledzenie KOL walletów
│   ├── smart_money.py  # Analiza historii walletów twórców
│   ├── volume_spike.py # Wykrywanie spikes po graduation
│   ├── rugcheck.py     # RugCheck.xyz client
│   ├── dexscreener.py  # DexScreener API client
│   ├── helius.py       # Helius RPC client
│   └── db.py           # SQLite persistence
├── run_v2.py           # Entry point
└── dashboard/          # Lokalne web UI
```

## Status

Live — @SignalTerminalBot | ~26% win rate, 362+ sygnałów

---
Built by [Emil Piński](https://emilpinski.pl)
