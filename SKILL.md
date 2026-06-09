---
name: DeFi Intelligence
description: Live token prices, gas and FX oracles, swap quotes, wallet analytics, DeFi position tracking, and ENS/Basename resolution for AI agents on Base. Read on-chain market and wallet intelligence pay-per-call via x402 — no API keys, no accounts. Powered by Spraay Protocol.
license: MIT
version: 1.0.0
homepage: https://github.com/plagtech/spraay-skills
---

# DeFi Intelligence

Read-only market and wallet intelligence for agents operating on Base. This skill wraps the **Spraay x402 Gateway** (`gateway.spraay.app`): price and gas oracles, swap quotes across Base DEXes, wallet profiling, transaction history, DeFi position tracking, and ENS/Basename resolution. Every call is paid per-use with x402 in USDC — no API keys, accounts, or subscriptions.

## When to use this skill

Use it when the agent needs to *understand* on-chain state before acting:
- Get a live token price, gas estimate, or stablecoin FX rate
- Quote a swap before executing one
- Profile a wallet — balances, top tokens, activity, age, risk signals
- Pull transaction history for an address
- Check open DeFi positions for an address
- Resolve an ENS name or Basename to an address (and back)

For *sending* value (batch pay, payroll, escrow) use the `agent-payments` skill in this repo. Note: `/api/v1/swap/execute` is included here for convenience, but a pure read agent can stop at `/swap/quote`.

## Setup

```bash
npm install x402-client
export EVM_PRIVATE_KEY=0x...   # wallet holding USDC on Base
```

```js
import { createClient } from "x402-client";
const client = createClient({
  baseUrl: "https://gateway.spraay.app",
  privateKey: process.env.EVM_PRIVATE_KEY,
});
```

The gateway answers an unpaid request with HTTP 402 and the price; the client pays and retries automatically.

## Endpoints

### Prices, gas & FX
| Method | Path | Price | Description |
|---|---|---|---|
| GET | `/api/v1/oracle/prices` | $0.008 | Aggregated oracle price feed across multiple sources. |
| GET | `/api/v1/oracle/gas` | $0.005 | Real-time gas prices for Base and other EVM chains. |
| GET | `/api/v1/oracle/fx` | $0.008 | Stablecoin FX rates: USDC, USDT, DAI, EURC, pyUSD, more. |
| GET | `/api/v1/prices` | $0.002 | Cached, low-latency multi-token price feed. |

### Wallet analytics
| Method | Path | Price | Description |
|---|---|---|---|
| GET | `/api/v1/analytics/wallet` | $0.01 | Wallet profile: balances, top tokens, activity tier, age, risk signals. |
| GET | `/api/v1/analytics/txhistory` | $0.008 | Transaction history for any address across supported chains. |
| GET | `/api/v1/balances` | $0.005 | Multi-chain balance lookup for any address. |
| GET | `/api/v1/tokens` | FREE | Supported tokens across all chains. |

### Swap (Base)
| Method | Path | Price | Description |
|---|---|---|---|
| GET | `/api/v1/swap/quote` | $0.008 | Swap quote across Uniswap V3, Aerodrome, and other Base DEXes. |
| GET | `/api/v1/swap/tokens` | $0.001 | Supported swap tokens with addresses, decimals, metadata. |
| POST | `/api/v1/swap/execute` | $0.015 | Execute a swap on Base via the MangoSwap router. |

### Positions & identity
| Method | Path | Price | Description |
|---|---|---|---|
| GET | `/api/v1/defi/positions` | $0.01 | Open DeFi positions for an address across supported protocols. |
| GET | `/api/v1/resolve` | $0.002 | Resolve an ENS, Basename, or address to its canonical identity. |

## Examples

Live prices and gas:
```js
const prices = await client.get("/api/v1/oracle/prices");   // { ETH: 2847.50, BTC: ... }
const gas = await client.get("/api/v1/oracle/gas");
```

Profile a wallet before interacting:
```js
const profile = await client.get("/api/v1/analytics/wallet?address=0xabc...");
```

Quote a swap:
```js
const quote = await client.get(
  "/api/v1/swap/quote?tokenIn=USDC&tokenOut=WETH&amount=100"
);
```

Resolve a Basename:
```js
const id = await client.get("/api/v1/resolve?name=jesse.base.eth");
```

## Notes
- Swaps route through Uniswap V3 / Aerodrome via the MangoSwap router on Base.
- Full catalog: https://docs.spraay.app · Live traffic: https://live.spraay.app
