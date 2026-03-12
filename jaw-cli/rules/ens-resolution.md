## ENS Resolution

When a user provides a name instead of a `0x` address (e.g. `hello.eth`, `alice.id`, `bob.com`), you MUST resolve it to an address before constructing any transaction.

ENS supports not just `.eth` names but any ICANN TLD that has been wrapped into ENS (`.id`, `.com`, `.xyz`, etc.). Because of this, do NOT filter by TLD — attempt resolution for any non-`0x` recipient and handle errors gracefully.

### How to resolve

Call the JustaName records API:

```
GET https://api.justaname.id/ens/v1/subname/records?ens={NAME}&providerUrl=https://eth.drpc.org
```

Example:

```bash
curl -s "https://api.justaname.id/ens/v1/subname/records?ens=hello.eth&providerUrl=https://eth.drpc.org"
```

### Response shape

```json
{
  "statusCode": 200,
  "result": {
    "data": {
      "ens": "hello.eth",
      "records": {
        "texts": [...],
        "coins": [
          { "id": 60, "value": "0xADDRESS", "name": "eth" },
          { "id": 2147492101, "value": "0xADDRESS", "name": "base" },
          ...
        ],
        "contentHash": null,
        "resolverAddress": "0x..."
      },
      "isJAN": false
    },
    "error": null
  }
}
```

### Extracting the address

Use the `coins` array to find the correct address for the target chain. Follow this priority:

1. **Default coinType:** First check for coin ID `2147483648` (`0x80000000`) — the ENSIP-11 default address. If present, use it regardless of chain.
2. **Chain-specific coinType:** If no default coin exists, look for a coin whose `id` matches `2147483648 + chainId` (ENSIP-11 formula for EVM chains). For example, Base (chain 8453) → coin ID `2147492101`.
3. **No match:** If neither exists, tell the user the name resolved but has no address set for the target chain.

Common coin IDs:

| Chain            | Chain ID  | Coin ID    | Coin name |
| ---------------- | --------- | ---------- | --------- |
| Default          | —         | 2147483648 | —         |
| Ethereum         | 1         | 60         | eth       |
| Base             | 8453      | 2147492101 | base      |
| Optimism         | 10        | 2147483658 | op        |
| Arbitrum         | 42161     | 2147525809 | arb1      |
| Polygon          | 137       | 2147483785 | matic     |
| BSC              | 56        | 2147483704 | bsc       |
| Linea            | 59144     | 2147542792 | linea     |
| Avalanche C      | 43114     | 2147526762 | avaxc     |
| zkSync           | 324       | 2147483972 | zksync    |
| Gnosis           | 100       | 2147483748 | gno       |
| Celo             | 42220     | 2147525868 | celo      |
| Scroll           | 534352    | 2148018000 | scr       |
| **Testnets**     |           |            |           |
| Sepolia          | 11155111  | 2158638759 | sep       |
| Base Sepolia     | 84532     | 2147568180 | basesep   |
| OP Sepolia       | 11155420  | 2158639068 | opsep     |
| Arbitrum Sepolia | 421614    | 2147905262 | arbsep    |
| Polygon Amoy     | 80002     | 2147563650 | polyamoy  |
| BSC Testnet      | 97        | 2147483745 | bsctest   |
| Linea Sepolia    | 59141     | 2147542789 | lineasep  |
| Avalanche Fuji   | 43113     | 2147526761 | avaxtest  |
| zkSync Sepolia   | 300       | 2147483948 | zktest    |
| Gnosis Chiado    | 10200     | 2147493848 | chiado    |
| Celo Alfajores   | 44787     | 2147528435 | alfajores |
| Scroll Sepolia   | 534351    | 2148017999 | scrsep    |

### Error handling

- **Name not found / no resolver:** The API returns `result.data` as `null` or `result.error` is non-null. Tell the user: "Could not resolve `{NAME}` — the name does not exist or has no resolver set."
- **No address for chain:** The `coins` array exists but contains neither the chain-specific coinType nor coinType `60`. Tell the user: "Resolved `{NAME}` but no address is set for the target chain."
- **API unreachable:** If the HTTP call fails, tell the user ENS resolution is unavailable and ask them to provide a `0x` address directly.

### Key rules

- You MUST attempt resolution for any recipient that is not a `0x` address — do NOT filter by TLD
- You MUST resolve BEFORE constructing the transaction — never pass a name to `wallet_sendCalls`
- You MUST check for the default coinType (`2147483648`) first, then fall back to the chain-specific coinType (`2147483648 + chainId`)
- You MUST NOT hardcode addresses — always resolve at call time, as ENS records can change
- You MUST show the user the resolved address before sending a transaction (e.g. "Resolved hello.eth to 0x1234…abcd on Base")
