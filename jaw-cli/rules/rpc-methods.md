# RPC Methods

## Method Categories

| Category | Browser Required | Description |
|----------|-----------------|-------------|
| Read-only | No | Direct API call or local session lookup |
| Local | No | Updates config/session on disk |
| Signing | Yes | Opens browser for passkey authentication |
| Session | Yes | Opens browser to establish wallet connection |

## All Supported Methods

| Method | Category | Browser | Description |
|--------|----------|---------|-------------|
| `wallet_connect` | Session | Yes | Connect wallet via passkey |
| `eth_requestAccounts` | Session | Yes | Connect wallet (alias) |
| `wallet_disconnect` | Local | No | Clear local session |
| `wallet_switchEthereumChain` | Local | No | Update chain ID in session |
| `wallet_sendCalls` | Signing | Yes | Send batched transaction(s) |
| `eth_sendTransaction` | Signing | Yes | Send single transaction |
| `personal_sign` | Signing | Yes | Sign a message |
| `eth_signTypedData_v4` | Signing | Yes | Sign EIP-712 typed data |
| `wallet_sign` | Signing | Yes | Unified signing method |
| `wallet_grantPermissions` | Signing | Yes | Grant delegated permissions |
| `wallet_revokePermissions` | Signing | Yes | Revoke permissions |
| `eth_accounts` | Read-only | No | Get connected address |
| `eth_chainId` | Read-only | No | Get current chain ID (hex) |
| `net_version` | Read-only | No | Get current chain ID (decimal) |
| `wallet_getCallsStatus` | Read-only | No | Check transaction batch status |
| `wallet_getCallsHistory` | Read-only | No | Get transaction history |
| `wallet_getAssets` | Read-only | No | Get wallet token balances |
| `wallet_getCapabilities` | Read-only | No | Get supported capabilities |
| `wallet_getPermissions` | Read-only | No | Get active permissions |

## Usage Examples

### Connect & Disconnect

```bash
jaw rpc call wallet_connect
jaw rpc call eth_accounts                # returns ["0xYourAddress"]
jaw rpc call wallet_disconnect
jaw rpc call eth_accounts                # returns []
```

### Transactions

```bash
# Send a transaction
jaw rpc call wallet_sendCalls '{"calls":[{"to":"0x...","value":"0x0","data":"0x..."}]}'

# Check status
jaw rpc call wallet_getCallsStatus '{"id":"0xBatchId"}'
```

### Signing

```bash
# Sign a message
jaw rpc call personal_sign '"Hello from JAW CLI"'

# Sign typed data (EIP-712)
jaw rpc call eth_signTypedData_v4 '["0xYOUR_ADDRESS", "{\"types\":{...},\"primaryType\":\"...\",\"domain\":{...},\"message\":{...}}"]'
```

### Permissions

```bash
# Grant permissions
jaw rpc call wallet_grantPermissions '{"expiry":1750000000,"spender":"0x...","permissions":{"calls":[{"target":"0x...","selector":"0xe0e0e0e0"}],"spends":[{"token":"0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE","allowance":"0x2386F26FC10000","unit":"day","multiplier":1}]}}'

# Check permissions
jaw rpc call wallet_getPermissions

# Revoke
jaw rpc call wallet_revokePermissions '{"id":"0xPERMISSION_ID"}'
```

### Chain Management

```bash
jaw rpc call eth_chainId                                        # "0x2105" (8453)
jaw rpc call wallet_switchEthereumChain '[{"chainId":"0x1"}]'   # switch to mainnet
jaw rpc call eth_chainId                                        # "0x1"
```

## Common Mistakes

- **personal_sign**: Pass message as a JSON string, not raw text: `'"Hello"'` not `'Hello'`
- **eth_signTypedData_v4**: Params must be an array: `'["0xAddr", "{...}"]'`
- **wallet_switchEthereumChain**: ChainId must be hex: `'[{"chainId":"0x2105"}]'` not `'[{"chainId":"8453"}]'`
- **wallet_sendCalls**: Wrap calls in object: `'{"calls":[...]}'` not `'[...]'`
