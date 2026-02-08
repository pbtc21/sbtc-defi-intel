# sBTC DeFi Intelligence API

Real-time analytics for programmable Bitcoin on Stacks.

## Overview

This API provides DeFi intelligence for sBTC, the trustless 1:1 Bitcoin peg on Stacks. It offers yield opportunity analysis, peg health monitoring, position simulation, and alpha signals for traders and autonomous agents.

Built on Cloudflare Workers with Hono, using x402 payment protocol for paid endpoints.

## Tech Stack

| Component | Technology |
|-----------|------------|
| Runtime | Bun |
| Framework | Hono |
| Platform | Cloudflare Workers |
| Language | TypeScript |
| Payments | x402 (STX or sBTC micropayments) |

## Features

- **sBTC Metrics**: Supply, holders, price, and volume tracking
- **Yield Opportunities**: APY, risk ratings, and TVL across DeFi protocols
- **Peg Health**: Real-time peg ratio, spread analysis, arbitrage detection
- **Position Simulation**: Preview outcomes before execution (Clarity decidability)
- **Agent Intelligence**: Batch endpoints optimized for autonomous trading
- **Alpha Signals**: Actionable opportunities with confidence scores

## API Endpoints

### Free Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /` | API info + interactive frontend |
| `GET /health` | Health check |
| `GET /overview` | sBTC ecosystem snapshot |

### Paid Endpoints

| Endpoint | Price | Description |
|----------|-------|-------------|
| `GET /yield-opportunities` | 2000 uSTX | Yield sources with APY and risk ratings |
| `GET /peg-health` | 2000 uSTX | Peg analysis and arbitrage detection |
| `GET /alpha` | 5000 uSTX | Actionable alpha signals |
| `POST /simulate` | 5000 uSTX | Position simulation before execution |
| `POST /agent-intel` | 5000 uSTX | Batch intelligence for autonomous agents |

Paid endpoints require `X-Payment` header with transaction ID. Supports both STX and sBTC payments.

## Quick Start

```bash
# Install dependencies
bun install

# Run locally
bun run dev

# Deploy to Cloudflare
bun run deploy
```

## Payment Integration

Paid endpoints use x402 protocol and accept both STX and sBTC:

### STX Payment
```
Contract: SPP5ZMH9NQDFD2K5CEQZ6P02AP8YPWMQ75TJW20M.simple-oracle
Function: call-with-stx
```

### sBTC Payment
Use query param `?tokenType=sBTC` or header `X-PAYMENT-TOKEN-TYPE: sBTC` to pay with sBTC.

| Endpoint | STX Price | sBTC Price |
|----------|-----------|------------|
| Standard endpoints | 2000 uSTX | 2 sats |
| Premium endpoints | 5000 uSTX | 5 sats |

Include the payment transaction ID in the `X-Payment` header.

## External Data Sources

| API | Purpose |
|-----|---------|
| [Hiro API](https://api.hiro.so) | Token metrics, transaction verification |
| [Tenero API](https://api.tenero.io) | Market data, prices |
| [CoinGecko](https://api.coingecko.com) | BTC price fallback |

## Supported Protocols

- **Zest Protocol** - Bitcoin-native lending
- **ALEX** - Leading Stacks DEX with sBTC pairs
- **Velar** - Multi-chain DEX with concentrated liquidity
- **StackingDAO** - Liquid staking for STX

## Project Structure

```
sbtc-defi-intel/
├── src/
│   └── index.ts      # Main API (all endpoints + frontend)
├── package.json      # Dependencies
├── wrangler.toml     # Cloudflare Worker config
└── ralph-docs.json   # Documentation state
```

## Why Stacks?

This API demonstrates what is uniquely possible with Stacks:

1. **Trustless BTC**: sBTC is a 1:1 peg via threshold signatures, not wrapped tokens
2. **Decidable Contracts**: Clarity enables position simulation without execution
3. **Native Micropayments**: x402 allows pay-per-call without accounts
4. **Bitcoin Finality**: Transactions settle on Bitcoin

## License

MIT
