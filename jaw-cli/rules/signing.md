## Signing

Sign messages and typed data using the connected passkey account. In default mode, signing requires browser interaction for passkey confirmation. In auto mode (`--session`), signing happens locally with the session key.

### Sign a plain message (personal_sign)

Params are `[message, address]` — message first, connected account address second.

```bash
# Sign a UTF-8 string (get address from eth_accounts)
ADDRESS=$(jaw rpc call eth_accounts -o json | jq -r '.[0]')
jaw rpc call personal_sign "[\"Hello, World!\", \"$ADDRESS\"]" -o json -y

# Inline if you already know your address
jaw rpc call personal_sign '["Hello, World!", "0xYOUR_ADDRESS"]' -o json -y

# Sign hex-encoded bytes
jaw rpc call personal_sign '["0x48656c6c6f2c20576f726c6421", "0xYOUR_ADDRESS"]' -o json -y
```

Returns a hex signature string.

### Sign EIP-712 typed data (eth_signTypedData_v4)

Params are `[address, typedDataJsonString]` — address first, typed data as a **JSON-encoded string** (not an object). Use `jq` to encode it:

```bash
ADDRESS=$(jaw rpc call eth_accounts -o json | jq -r '.[0]')
TYPED_DATA='{"domain":{"name":"MyApp","version":"1","chainId":8453,"verifyingContract":"0xCONTRACT"},"types":{"Mail":[{"name":"from","type":"address"},{"name":"content","type":"string"}]},"primaryType":"Mail","message":{"from":"0xSENDER","content":"Hello"}}'

jaw rpc call eth_signTypedData_v4 \
  "$(jq -n --arg addr "$ADDRESS" --argjson td "$TYPED_DATA" '[$addr, ($td | tojson)]')" \
  -o json -y
```

Returns a hex signature string.

### Sign with wallet_sign (ERC-7871 unified method)

`wallet_sign` accepts a `request` object with a `type` field:
- `"0x45"` for personal sign (UTF-8 message)
- `"0x01"` for EIP-712 typed data

```bash
# Personal sign via wallet_sign
jaw rpc call wallet_sign \
  '{"request":{"type":"0x45","data":{"message":"Hello, World!"}}}' \
  -o json -y

# EIP-712 typed data via wallet_sign
jaw rpc call wallet_sign \
  '{"request":{"type":"0x01","data":{"domain":{"name":"MyApp","version":"1","chainId":8453},"types":{"Message":[{"name":"content","type":"string"}]},"primaryType":"Message","message":{"content":"Hello"}}}}' \
  -o json -y
```

Returns a hex signature string.

### Sign with auto mode (no browser)

With `--session`, signing uses the local session key — no browser needed:

```bash
jaw rpc call personal_sign '["Hello from agent", "0xSESSION_ADDRESS"]' --session -o json -y
jaw rpc call eth_signTypedData_v4 "..." --session -o json -y
```

Note: `wallet_sign` is not supported in session mode. Use `personal_sign` or `eth_signTypedData_v4`.

### Key rules

- In default mode, you MUST keep the browser tab open — signing requires passkey confirmation
- `personal_sign` params are `[message, address]` — always pass both; use `eth_accounts` to get the address
- `eth_signTypedData_v4` params are `[address, typedDataJsonString]` — the typed data must be a JSON-encoded **string**, not an object; use `jq` to encode: `($td | tojson)`
- `wallet_sign` uses `{request:{type:"0x45"|"0x01", data:{...}}}` — not `{account, data}`
- You MUST pass the full EIP-712 typed data object to `eth_signTypedData_v4` — domain, types, primaryType, and message are all required
