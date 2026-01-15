# src - API Source Code

Main source directory containing the Hono API implementation.

## Files

### index.ts

Single-file API containing:

- **Configuration**: Payment contract, API endpoints, protocol definitions
- **Data Fetchers**: Functions to retrieve sBTC metrics, protocol data, peg health
- **Payment Verification**: On-chain transaction verification via Hiro API
- **Alpha Signal Generation**: Algorithmic opportunity detection
- **Frontend**: Embedded HTML/CSS/JS for interactive demo
- **Route Handlers**: All endpoint implementations

## Architecture

```
index.ts
├── Configuration
│   ├── CONTRACT (payment settings)
│   ├── SBTC (token addresses)
│   └── PROTOCOLS (DeFi protocol registry)
│
├── Helper Functions
│   ├── paymentRequired() - 402 response generator
│   ├── verifyPayment() - On-chain tx verification
│   ├── fetchBtcPrice() - BTC price from CoinGecko
│   ├── fetchSbtcMetrics() - Supply, holders, price
│   ├── fetchProtocolData() - Protocol TVL and APY
│   ├── calculatePegHealth() - Peg ratio and status
│   └── generateAlphaSignals() - Opportunity detection
│
├── Frontend
│   └── getFrontendHtml() - Interactive demo UI
│
└── Routes
    ├── GET /          - API info + frontend
    ├── GET /health    - Health check
    ├── GET /overview  - Free ecosystem snapshot
    ├── GET /yield-opportunities - Paid yield data
    ├── GET /peg-health - Paid peg analysis
    ├── GET /alpha     - Paid alpha signals
    ├── POST /simulate - Paid position simulation
    └── POST /agent-intel - Paid agent batch data
```

## Key Concepts

### Payment Flow

1. Client calls paid endpoint without `X-Payment` header
2. API returns 402 with payment instructions
3. Client sends STX to payment contract
4. Client retries with `X-Payment: <txid>` header
5. API verifies transaction on-chain and returns data

### Position Simulation

The `/simulate` endpoint leverages Clarity's decidability:

```typescript
// Outcomes computed via read-only calls
// No gas required, deterministic results
simulation.claritySimulation = {
  verified: true,
  method: 'read-only-call',
  note: 'Outcome computed via Clarity contract read'
};
```

### Alpha Signals

Generated from:
- Yield differentials across protocols
- Peg discount/premium opportunities
- TVL concentration warnings
- Risk-adjusted recommendations

## Data Sources

| Source | Used For |
|--------|----------|
| Hiro API | Token supply, holder count, tx verification |
| Tenero API | Price data, volume, market metrics |
| CoinGecko | BTC price fallback |

## Protocols Tracked

| Protocol | Type | Contract |
|----------|------|----------|
| Zest Protocol | Lending | `SP2VCQJGH7PHP2DJK7Z0V48AGBHQAW3R3ZW1QF4N.pool-vault` |
| ALEX | DEX | `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.alex-vault` |
| Velar | DEX | `SP1Y5YSTAHZ88XYK1VPDH24GY0HPX5J4JECTMY4A1.velar-v2` |
| StackingDAO | Liquid Staking | `SP4SZE494VC2YC5JYG7AYFQ44F5Q4PYV7DVMDPBG.stacking-dao-core-v1` |

## Development

```bash
# Run locally with hot reload
bun run dev

# Type check
bunx tsc --noEmit

# Deploy
bun run deploy
```

The API runs on Cloudflare Workers at the edge, providing low-latency responses globally.
