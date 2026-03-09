# MCP Server

## Starting the Server

```bash
jaw mcp
```

Runs over stdio — meant to be called by an MCP client, not used interactively.

## Available Tools

### `jaw_rpc` — Execute any EIP-1193 RPC method

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `method` | string | Yes | EIP-1193 RPC method name |
| `params` | any | No | Method parameters (varies by method) |
| `chainId` | number | No | Target chain ID (overrides default) |

### `jaw_config_show` — Show current configuration

Returns current CLI config with API key redacted. No parameters.

### `jaw_config_set` — Set a configuration value

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `key` | enum | Yes | One of: `apiKey`, `defaultChain`, `keysUrl`, `paymasterUrl` |
| `value` | string | Yes | Value to set |

## Agent Usage Examples

### Connect and send transaction

```
1. jaw_rpc({ method: "wallet_connect" })
   → Opens browser, user signs with passkey
   → Returns ["0xUserAddress"]

2. jaw_rpc({ method: "wallet_sendCalls", params: { calls: [{ to: "0x...", value: "0x0" }] } })
   → Opens browser for signing
   → Returns batch ID

3. jaw_rpc({ method: "wallet_getCallsStatus", params: { id: "0xBatchId" } })
   → No browser needed
   → Returns status
```

### Read-only queries (no browser)

```
jaw_rpc({ method: "eth_accounts" })           → ["0xAddress"] or []
jaw_rpc({ method: "eth_chainId" })            → "0x2105"
jaw_rpc({ method: "wallet_getAssets" })        → token balances
jaw_rpc({ method: "wallet_getPermissions" })   → active permissions
jaw_rpc({ method: "wallet_getCapabilities" })  → supported features
```

### Chain management

```
jaw_rpc({ method: "wallet_switchEthereumChain", params: [{ chainId: "0x1" }] })
jaw_config_set({ key: "defaultChain", value: "8453" })
```

## Testing the MCP Server

Pipe JSON-RPC to verify it works:

```bash
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}' | jaw mcp
```
