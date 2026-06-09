# DeFi Intelligence (x402)

Live token prices, gas/FX oracles, swap quotes, wallet analytics, DeFi positions, and ENS/Basename resolution for AI agents on **Base**. Pay-per-call via [x402](https://x402.org): no API keys, no accounts.

## Install

```bash
npx skills add plagtech/defi-intelligence-x402
```

## What it does

Read on-chain market and wallet intelligence before an agent acts — prices, gas, swap quotes across Base DEXes, wallet profiling, transaction history, and identity resolution. Full endpoint list and examples are in [`SKILL.md`](./SKILL.md).

## Payment

```bash
npm install x402-client
export EVM_PRIVATE_KEY=0x...   # wallet holding USDC on Base
```

Powered by the [Spraay x402 Gateway](https://gateway.spraay.app) · [Docs](https://docs.spraay.app) · [Live](https://live.spraay.app)
