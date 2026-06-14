# Token Check

Pull a live snapshot of a token or wallet through the Spraay x402 gateway.

Use the `spraay-x402` MCP server (read-only — no funds move).

## Steps

1. Identify what the user wants a snapshot of:
   - **A token** → fetch current price, and if useful, a swap quote and 24h context.
   - **A wallet/address** → fetch balances, holdings, and a short activity profile.
   - Resolve any ENS name or Basename to an address first if one was given.
2. Pull the data live from Spraay — never report a price or balance from memory.
3. Summarize concisely: the key numbers, the chain, and the timestamp/"as of now".
4. If the user is weighing a trade, add a swap quote (output + price impact) but
   stop there — this command does not execute swaps or move funds.

## Notes

- Default to Base; offer Ethereum or Solana if the user asks.
- Reads are metered per-call via x402 — no API key needed.
- For anything that moves funds (swaps, payouts), point the user to the
  execution tools rather than acting here.
