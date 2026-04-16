## ENS Resolution

When a user provides a name instead of a `0x` address (e.g. `hello.eth`, `alice.id`, `bob.com`), you MUST resolve it to an address before constructing any transaction.

ENS supports not just `.eth` names but any ICANN TLD that has been wrapped into ENS (`.id`, `.com`, `.xyz`, etc.). Because of this, do NOT filter by TLD — attempt resolution for any non-`0x` recipient and handle errors gracefully.

### How to resolve

Use the interoperable name format (ERC-7828) to resolve a name to an address on a specific chain in a single call. Pass `name@chain` in the `ens` parameter:

```
GET https://api.justaname.id/ens/v1/subname/records?ens={NAME}@{CHAIN}&providerUrl=https://eth.drpc.org
```

The `{CHAIN}` can be:
- A human-readable label: `ethereum`, `arbitrum`, `optimism`, `base`
- A CAIP-2 identifier: `eip155:1`, `eip155:42161`, `eip155:10`, `eip155:8453`

Examples:

```bash
# Resolve hello.eth on Base
curl -s "https://api.justaname.id/ens/v1/subname/records?ens=hello.eth@base&providerUrl=https://eth.drpc.org"

# Resolve hello.eth on Arbitrum using CAIP format
curl -s "https://api.justaname.id/ens/v1/subname/records?ens=hello.eth@eip155:42161&providerUrl=https://eth.drpc.org"

# Resolve hello.eth on Ethereum
curl -s "https://api.justaname.id/ens/v1/subname/records?ens=hello.eth@ethereum&providerUrl=https://eth.drpc.org"
```

### Response shape

The response contains only the address for the requested chain:

```json
{
  "statusCode": 200,
  "result": {
    "data": {
      "ens": "hello.eth",
      "records": {
        "texts": [...],
        "addresses": [
          { "id": 2147492101, "value": "0xADDRESS", "name": "base" }
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

```
addresses[0].value  →  the resolved 0x address for the target chain
```

If `addresses` is empty, the name has no address set for the target chain.

### Determining the chain identifier

Use the chain the user's wallet is connected to. Common chain identifiers:

| Chain            | Human label  | CAIP format       |
| ---------------- | ------------ | ----------------- |
| Ethereum         | `ethereum`   | `eip155:1`        |
| Base             | `base`       | `eip155:8453`     |
| Optimism         | `optimism`   | `eip155:10`       |
| Arbitrum         | `arbitrum`   | `eip155:42161`    |

For chains not listed above, use the CAIP format `eip155:{chainId}` which works for any EVM chain.

### Fetching all records (no chain filter)

To get all records without chain filtering (e.g. for displaying a profile), omit the `@chain` part:

```bash
curl -s "https://api.justaname.id/ens/v1/subname/records?ens=hello.eth&providerUrl=https://eth.drpc.org"
```

This returns all addresses, text records, and content hash — the same behavior as before.

### Error handling

- **Name not found / no resolver:** The API returns `result.data` as `null` or `result.error` is non-null. Tell the user: "Could not resolve `{NAME}` — the name does not exist or has no resolver set."
- **No address for chain:** The `addresses` array is empty. Tell the user: "Resolved `{NAME}` but no address is set for the target chain."
- **Unsupported chain label:** The API returns a 400 error with a message like "Unsupported chain label". Tell the user the chain is not recognized and suggest using CAIP format (`eip155:{chainId}`).
- **API unreachable:** If the HTTP call fails, tell the user ENS resolution is unavailable and ask them to provide a `0x` address directly.

### Key rules

- You MUST attempt resolution for any recipient that is not a `0x` address — do NOT filter by TLD
- You MUST resolve BEFORE constructing the transaction — never pass a name to `wallet_sendCalls`
- You MUST use the interoperable name format (`name@chain`) to resolve the address for the correct chain
- You MUST NOT hardcode addresses — always resolve at call time, as ENS records can change
- You MUST show the user the resolved address before sending a transaction (e.g. "Resolved hello.eth to 0x1234…abcd on Base")
