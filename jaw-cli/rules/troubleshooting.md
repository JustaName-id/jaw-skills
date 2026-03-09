# Troubleshooting

## Common Errors

### "Unsupported method: ..."

The method name is not in the supported list. Check spelling — methods are case-sensitive.

Supported: `wallet_connect`, `eth_requestAccounts`, `wallet_disconnect`, `wallet_switchEthereumChain`, `wallet_sendCalls`, `eth_sendTransaction`, `personal_sign`, `eth_signTypedData_v4`, `wallet_sign`, `wallet_grantPermissions`, `wallet_revokePermissions`, `eth_accounts`, `eth_chainId`, `net_version`, `wallet_getCallsStatus`, `wallet_getCallsHistory`, `wallet_getAssets`, `wallet_getCapabilities`, `wallet_getPermissions`

### "API key required"

Set your API key via one of:
```bash
jaw config set apiKey YOUR_KEY
export JAW_API_KEY=YOUR_KEY
jaw rpc call <method> --api-key YOUR_KEY
```

### "Invalid JSON params"

Params must be valid JSON. Common mistakes:
```bash
# WRONG — unquoted string
jaw rpc call personal_sign Hello

# CORRECT — JSON string
jaw rpc call personal_sign '"Hello"'

# CORRECT — JSON object
jaw rpc call wallet_sendCalls '{"calls":[{"to":"0x...","value":"0x0"}]}'
```

### "Request timed out after 120s"

The browser action wasn't completed within the timeout. Check:
- Was the browser window opened? (May need to allow popups)
- Did you complete the passkey signing?
- Increase timeout: `jaw rpc call <method> --timeout 300`

### "Callback URL must be localhost"

The bridge page validates that callbacks only go to `127.0.0.1` or `localhost`. This error means the callback URL was tampered with.

### "Untrusted keysUrl"

The `keysUrl` config value must be a `*.jaw.id` domain (HTTPS) or localhost. Reset it:
```bash
jaw config set keysUrl https://keys.jaw.id
```

### Browser doesn't open

- Make sure you're not in an SSH or headless environment
- On macOS, `open` should work automatically
- On Linux, ensure `xdg-open` is available and a display server is running

### eth_accounts returns []

No active session. Run `jaw rpc call wallet_connect` first.

### wallet_switchEthereumChain errors

ChainId must be hex:
```bash
# WRONG
jaw rpc call wallet_switchEthereumChain '[{"chainId":"8453"}]'

# CORRECT
jaw rpc call wallet_switchEthereumChain '[{"chainId":"0x2105"}]'
```

## Debugging Tips

1. **Check config**: `jaw config show --output json`
2. **Check session**: `cat ~/.jaw/session.json`
3. **Use JSON output**: `jaw rpc call <method> --output json` for machine-readable output
4. **Verbose errors**: Error messages include JSON-RPC error codes when available
5. **Test MCP**: `echo '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}' | jaw mcp`
