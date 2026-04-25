# Signal Terminal — Solana Signal Bot

> Real-time alpha signals for Solana pump.fun tokens.

**Status:** Live

## What it does
- Tracks KOL (Key Opinion Leader) wallet activity on Solana
- Detects volume spikes and momentum patterns on pump.fun / PumpSwap
- Delivers structured Telegram alerts with entry price, market cap, liquidity, buy/sell ratio
- 362 signals tracked since launch; average peak multiplier 5.4x

## Tech Stack
`Python` `aiohttp` `asyncio` `DexScreener API` `Solana RPC` `Telegram Bot API`

## Pipeline
Market observer → signal scorer → filter engine → Telegram delivery
